<template>
  <body>
    <v-app>
      <v-container>
        <TabelaComponent
          titulo="Tabelasso"
          :items="items"
          :headers="headers"
          @editou="editItem"
          @deletou="deleteItem"
          @abrir-dialog="() => ativo = true"
        />
        <v-dialog v-model="ativo" max-width="500">
          <v-card height="550" width="500" theme="dark">
            <v-card-title>
              {{ tituloDialog }}
            </v-card-title>
            <v-card-text>
              <v-row>
                <v-col>
                  <v-text-field
                    variant="outlined"
                    label="Nome"
                    placeholder="nome"
                    v-model="product.name"
                  >
                  </v-text-field>
                </v-col>
                <v-col>
                  <v-text-field
                    variant="outlined"
                    label="Preço"
                    placeholder="price"
                    v-model="product.price"
                  >
                  </v-text-field>
                </v-col>
              </v-row>

              <v-row>
                <v-col>
                  <img
                  :src="product.image"
                  style="max-width: 100px; max-height: 100px"
                  >
                </v-col>
                
                <v-col 
                  class="justify-center align-center text-center"
                  cols="8"
                  sm="6"
                  style="justify-content: center;"
                >
                  <v-text-field v-model="product.image" label="Imagem" />
                </v-col>
              </v-row>

              <v-row>
                <v-col>
                  <v-text-field
                    variant="outlined"
                    label="Descrição"
                    placeholder="descricione"
                    v-model="product.description"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col>
                  <v-autocomplete
                    :items="category"
                    item-title="name"
                    item-value="id"
                    v-model="product.idCategory"
                  >
                  </v-autocomplete>

                </v-col>
              </v-row>
              <v-row></v-row>
            </v-card-text>
            <v-card-actions>
              <v-btn variant="outlined" class="text-none" @click="persist()">
                Enviar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-container>
    </v-app>
  </body>
</template>

<script>
export default {
  data: () => {
    return {
      valor: 0,
      ativo: false,
      loading: true,
      textoUsuario: null,
      product: {
        name: null,
        price: null,
        image: null,
        description: null,
        idCategory: null,
      },
      search: "",
      headers: [
        {
          title: "Identificador",
          key: "id",
        },
        {
          title: "Nomes",
          key: "name",
        },
        {
          title: "Preço",
          key: "price",
        },
        {
          title: "action",
          key: "action",
          sortable: false,
        },
      ],
      items: [],
      category: [],
    };
  },

  async created() {
    await this.getItems();
    await this.getCategory();
  },

  computed: {
    tituloDialog: function () {
      return this.product.id ? "Editar" : "Criar";
    },
  },

  watch: {
    ativo(valor) {
      if (valor == false) {
        this.resetProduct();
      }
    },
  },

  methods: {
    async persist() {
      if (this.product.id) {
        const response = await this.$api.patch(`/product/${this.product.id}`, this.product);
      } else {
        const response = await this.$api.post("/product", this.product);
      }
      this.resetProduct();
      await this.getItems();
    },

    editItem(item) {
      this.product = {
        ...item,
      };
      this.ativo = true;
    },

    resetProduct() {
      this.product = {
        nome: null,
        status: null,
      };
      this.ativo = false;
    },

    async getItems() {
      const response = await this.$api.get("/product");
      this.items = response.response;
      return response;
    },

    async getCategory() {
      const response = await this.$api.get("/category");
      this.category = response.response;
      return response;
    },

    mudaPagina() {
      this.$router.push({ path: "/pagina" });
    },

    async deleteItem(item) {
      if (confirm(`Deseja deletar?`)) {
        const response = await this.$api.delete(`/product/destroy/${item.id}`);
      }
      if (response.type == "error") {
        alert(response.message);
      }
    },

  },
};
</script>
