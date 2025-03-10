
<template class="degrade">
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
                      <img :src="product.image" style="max-width: 100px; max-height: 100px">
                    </v-col>
                    <v-col class="justify-center align-center text-center" cols="8" sm="6" style="justify-content: center;">
                      <v-text-field v-model="product.image" label="Link da imagem" />
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
        try {
          let response;
          if (this.product.id) {
            response = await this.$api.patch(`/product/persist/${this.product.id}`, this.product);
          } else {
            response = await this.$api.post("/product/persist", this.product);
          }
          if (response.type == "error") {
            alert(response.message);
          } else {
            this.resetProduct();
            await this.getItems();
          }
        } catch (error) {
          console.error("Error persisting product:", error);
          alert("Ocorreu um erro ao salvar o produto.");
        }
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
        try {
          const response = await this.$api.get("/product");
          this.items = response.response;
        } catch (error) {
          console.error("Error fetching items:", error);
          alert("Ocorreu um erro ao buscar os produtos.");
        }
      },
  
      async getCategory() {
        try {
          const response = await this.$api.get("/category");
          this.category = response.response;
        } catch (error) {
          console.error("Error fetching categories:", error);
          alert("Ocorreu um erro ao buscar as categorias.");
        }
      },
  
      mudaPagina() {
        this.$router.push({ path: "/pagina" });
      },
  
      async deleteItem(item) {
        if (confirm(`Deseja deletar?`)) {
          try {
            const response = await this.$api.delete(`/product/delete/${item.id}`);
            if (response.type == "error") {
              alert(response.message);
            } else {
              this.resetProduct();
              await this.getItems();
            }
          } catch (error) {
            console.error("Error deleting item:", error);
            alert("Ocorreu um erro ao deletar o produto.");
          }
        }
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
  