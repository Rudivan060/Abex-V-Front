<template>
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

  <body class="degrade">
    <v-container>
      <v-row>
        <v-col v-for="(item, i) in items" :key="i" cols="12" md="4">
          <v-card class="mx-auto" max-width="344" style="border: solid black 4px" elevation="24">
            <v-card-item>
              <div>
                <div class="text-h6 mb-1">Código: {{ item.code }}</div>
              </div>
              <div>
                <div class="text-h6 mb-1">Valor: {{ item.value }}{{ item.type }} </div>
              </div>
              <div>
                <div class="text-h6 mb-1"> Disponíveis: {{ item.use }}</div>
              </div>
            </v-card-item>
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
      cupom: {
        code: null,
        type: null,
        value: null,
        use: null,
      },
      tab: null,
      search: "",
      tab: 2,
      show: false,
      items: [],
      quantidade: '',
      observacoes: '',
      rules: [
        value => {
          if (value) return true

          return 'Deve inserir todas as informações!'
        },
      ],
    };
  },

  async created() {
    await this.getItems();
  },

  methods: {
    async getItems() {
      const response = await this.$api.get("/cupom");
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
      const response = await this.$api.post("/cupom/persist", this.cupom);
      this.resetCupom();
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
