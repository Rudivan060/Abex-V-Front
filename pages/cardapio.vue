<template>
  <template>
    <v-card style="background-color:  #fb911f;">
      <v-tabs v-model="tab" align-tabs="center" color="red">
        <a href="http://localhost:3000/ademiro">
          <v-icon class="bah">
            mdi-account-lock
          </v-icon>
        </a>

        <a href="http://localhost:3000/">
          <v-tab :value="1" @click="" style="color: darkred; font-weight: bolder; 
          font-size:larger;" class="text-none">Início</v-tab>
        </a>  

        <a href="https://www.youtube.com/watch?v=ycHVUvvOwzY">
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

  <body class="degrade">
    <v-container>
      <v-row>
        <v-col v-for="(item, i) in items" :key="i" cols="12" md="4">
          <v-card
            class="mx-auto"
            max-width="344"
            style="border: solid black 4px"
            elevation="24"
          >
            <v-img height="200px" :src="item.image" cover></v-img>
            <v-card-item>
              <div>
                <div class="text-h6 mb-1">{{ item.name }}</div>
              </div>
            </v-card-item>

            <v-card-actions>
              <v-btn 
                color="orange-lighten-2" 
                text
                @click="abrirDialog()"
                >
                Encomendar
              </v-btn>
              <v-spacer></v-spacer>
              <v-btn
                :icon="item.ativo ? 'mdi-chevron-up' : 'mdi-chevron-down'"
                @click="item.ativo = !item.ativo"
              ></v-btn>
            </v-card-actions>

            <v-expand-transition>
              <div v-show="item.ativo">
                <v-divider></v-divider>
                <v-card-text> R$ {{ item.price }} </v-card-text>

                <v-card-text>
                  Ingredientes: {{ item.description }}
                </v-card-text>
              </div>
            </v-expand-transition>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
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
      tab: 3,
      show: false,
      items: [],
    };
  },

  async created() {
    await this.getItems();
  },

  methods: {
    async getItems() {
      const response = await this.$api.get("/product");
      this.items = response.response;
      this.items = this.items.map((item) => {
        return {
          ativo: false,
          ...item,
        };
      });
      return response;
    },

    async create() {
      const response = await this.$api.post("/product/persist", this.product);
      this.resetProduct();
      await this.getItems();
    },
  },
};
</script>

<style scoped>
  .text-none {
    text-decoration: none;
  }

  .degrade {
    background: rgb(2, 0, 36);
    background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
    background-repeat: no-repeat;
    height: 100dvh;
  }
</style>
