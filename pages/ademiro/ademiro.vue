<template class="degrade">
  <template>
    <v-card style="background-color:  #fb911f;">
      <v-tabs v-model="tab" align-tabs="center" color="red">
        <a href="http://localhost:3000/ademiro/ademiro">
          <v-icon class="bah">
            mdi-account-lock
          </v-icon>
        </a>

        <a href="http://localhost:3000/">
          <v-tab :value="1" @click="" style="color: darkred; font-weight: bolder; 
          font-size:larger;" class="text-none">Início</v-tab>
        </a>

        <a href="http://localhost:3000/cupom">
          <v-tab :value="2" @click="" style="color: darkred; font-weight: bold; 
          font-size:large;" class="text-none">Cupons</v-tab>
        </a>

        <a href="http://localhost:3000/cardapio">
          <v-tab :value="3" @click="" style="color: darkred; font-weight: bold;
          font-size:large;" class="text-none">Cardápio</v-tab>
        </a>

        <a href="https://www.youtube.com/watch?v=hvL1339luv0">
          <v-tab :value="4" @click="" style="color: darkred; font-weight: bold;
          font-size:large;" class="text-none">AppBK</v-tab>
        </a>

        <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ">
          <v-tab :value="5" @click="" style="color: darkred; font-weight: bold;
          font-size:large;" class="text-none">ClubeBK</v-tab>
        </a>

        <a href="http://localhost:3000/login">
          <v-icon class="bah" style="position: relative; width: 50%;">
            mdi-login
          </v-icon>
        </a>

      </v-tabs>
    </v-card>
  </template>

  <div>
    <v-app>
      <v-container>
        <TabelaComponent titulo="Tabelasso" :items="items" :headers="headers" @editou="editItem" @deletou="deleteItem"
          @abrir-dialog="() => ativo = true" />
        <v-dialog v-model="ativo" max-width="500">
          <v-card height="550" width="500" theme="dark">
            <v-card-title>
              {{ tituloDialog }}
            </v-card-title>
            <v-card-text>
              <v-row>
                <v-col>
                  <v-text-field variant="outlined" label="Nome" placeholder="nome" v-model="user.name">
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
                  <v-text-field variant="outlined" label="Descrição" placeholder="descricione"
                    v-model="product.description">
                  </v-text-field>
                </v-col>
              </v-row>

              <v-row>
                <v-col>
                  <v-autocomplete :items="category" item-title="name" item-value="id" v-model="product.idCategory">
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
  </div>

  <v-card>
    <v-layout>
      <v-navigation-drawer expand-on-hover rail theme="dark">
        <v-list>
          <v-list-item prepend-avatar="https://randomuser.me/api/portraits/women/85.jpg" subtitle="sandra_a88@gmailcom"
            title="Sandra Adams"></v-list-item>
        </v-list>

        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-home" to="/" title="Início" value="starred"></v-list-item>
          <v-list-item href="http://localhost:3000/ademiro/crudCupom" prepend-icon="mdi-ticket-percent-outline"
            title="Cardápio" value="myfiles"></v-list-item>
          <v-list-item href="http://localhost:3000/ademiro/crudAddress" prepend-icon="mdi-google-maps" title="Endereço"
            value="localizacao"></v-list-item>
          <v-list-item href="http://localhost:3000/ademiro/crudPayment" prepend-icon="mdi-credit-card-outline"
            title="pagamentos" value="opcao"></v-list-item>
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
          name: null,
          price: null,
          image: null,
          description: null,
          idCategory: null,
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

<style>
  .degrade {
    background: rgb(2, 0, 36);
    background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
    background-repeat: no-repeat;
    height: 100dvh;
  }
</style>
