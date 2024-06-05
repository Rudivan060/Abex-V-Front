<template>
  <template>
    <v-card style="background-color: orangered">
      <v-tabs v-model="tab" align-tabs="center" color="deep-purple-accent-4">
        <a href="http://localhost:3000">
          <v-tab
            :value="1"
            @click=""
            style="color: cadetblue; font-weight: bolder; font-size: larger"
            class="text-none"
            >Início</v-tab
          >
        </a>

        <a href="">
          <v-tab
            :value="2"
            @click=""
            style="color: cadetblue; font-weight: bold; font-size: large"
            class="text-none"
            >Cupons</v-tab
          >
        </a>

        <a href="http://localhost:3000/cardapio">
          <v-tab
            :value="3"
            @click=""
            style="color: cadetblue; font-weight: bold; font-size: large"
            class="text-none"
            >Cardápio</v-tab
          >
        </a>

        <a href="">
          <v-tab
            :value="4"
            @click=""
            style="color: cadetblue; font-weight: bold; font-size: large"
            class="text-none"
            >AppBK</v-tab
          >
        </a>

        <a href="">
          <v-tab
            :value="5"
            @click=""
            style="color: cadetblue; font-weight: bold; font-size: large"
            class="text-none"
            >ClubeBK</v-tab
          >
        </a>

        <a href="">
          <v-tab
            :value="6"
            @click=""
            style="color: cadetblue; font-weight: bold; font-size: large"
            class="text-none"
            >Delivery</v-tab
          >
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
  }
</style>
