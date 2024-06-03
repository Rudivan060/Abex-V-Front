<template>
  
  <template style="background-color: bisque;">
    <v-card style="background-color: #f5ebdc;">
      <v-tabs v-model="tab" align-tabs="center" color="deep-purple-accent-4">
        <v-tab :value="1" @click="" style="color: cadetblue;font-weight:
             bolder; font-size:larger;" href="http://localhost:3000/" class="text-none">Início</v-tab>

        <v-tab :value="2" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none" >Cupons</v-tab>

        <v-tab :value="3" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none" href="http://localhost:3000/cardapio">Cardápio</v-tab>

        <v-tab :value="4" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">App BK</v-tab>

        <v-tab :value="5" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">Clube BK</v-tab>

        <v-tab :value="6" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">Delivery</v-tab>
      </v-tabs>
    </v-card>
  </template>

  <v-app>
    <v-container style="border-radius: 6%;">
      <TabelaComponent titulo="Tabelasso" 
        :items="items" 
        :headers="headers" 
        @editou="editItem" 
        @deletou="deleteItem"
        @abrir-dialog="() => ativo = true" />
      <v-dialog v-model="ativo" max-width="500">
        <v-card height="550" width="500" theme="dark">
          <v-card-title>
            {{ tituloDialog }}
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col>
                <v-text-field variant="outlined" label="Nome" placeholder="nome" v-model="product.name">
                </v-text-field>
              </v-col>
              <v-col>
                <v-text-field variant="outlined" label="Preço" placeholder="price" v-model="product.price">
                </v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <img :src="product.image" style="max-width: 100px; max-height: 100px">
              </v-col>
              <v-col class="justify-center align-center text-center" cols="8" sm="6" style="justify-content: center;">
                <v-text-field v-model="product.image" label="Link da imagem" />
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <v-text-field 
                  variant="outlined" 
                  label="Descrição" 
                  placeholder="descricione"
                  v-model="product.description">
                </v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <v-autocomplete 
                :items="category" 
                item-title="name" 
                item-value="id" 
                v-model="product.idCategory">
                </v-autocomplete>
              </v-col>
            </v-row>
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

  <v-card>
    <v-layout>
      <v-navigation-drawer
        expand-on-hover
        rail
        theme="dark"
      >
        <v-list>
          <v-list-item
            prepend-avatar="https://randomuser.me/api/portraits/women/85.jpg"
            subtitle="sandra_a88@gmailcom"
            title="Sandra Adams"
          ></v-list-item>
        </v-list>

        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-home" to="/" title="Início" value="starred"></v-list-item>
          <v-list-item href="http://localhost:3000/cardapio" prepend-icon="mdi-food" title="Cardápio" value="myfiles"></v-list-item>
          <v-list-item prepend-icon="mdi-account-multiple" title="Shared with me" value="shared"></v-list-item>
          <v-list-item prepend-icon="mdi-star" title="Starred" value="starred"></v-list-item>
        </v-list>
      </v-navigation-drawer>
      <v-main style="background-color: black;" theme="dark">
        <slot/>
      </v-main>
    </v-layout>
  </v-card>
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
        tab: null,
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
          const response = await this.$api.patch(`/product/persist/${this.product.id}`, this.product);
        } else {
          const response = await this.$api.post("/product/persist", this.product);
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
          const response = await this.$api.delete(`/product/delete/${item.id}`);
        }
        if (response.type == "error") {
          alert(response.message);
        }
        this.resetProduct();
      },

    },
  };
</script>
