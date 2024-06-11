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

  <body clas>
    <v-app>
      <v-container style="border-radius: 6%;">
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
                  <v-text-field variant="outlined" label="Código" placeholder="Código" v-model="cupom.code">
                  </v-text-field>
                </v-col>

                <v-col>
                  <v-text-field variant="outlined" label="Tipo" placeholder="Tipo" v-model="cupom.type">
                  </v-text-field>
                </v-col>
              </v-row>

              <v-row>
                <v-col>
                  <v-text-field variant="outlined" label="Valor" placeholder="Valor" v-model="cupom.value">
                  </v-text-field>
                </v-col>

                <v-col>
                  <v-text-field variant="outlined" label="Quantidade" placeholder="Quantidade" v-model="cupom.use">
                  </v-text-field>
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
  </body>

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
        <slot />
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
      cupom: {
        code: null,
        type: null,
        value: null,
        use: null,
      },
      tab: null,
      search: "",
      headers: [
        {
          title: "Identificador",
          key: "id",
        },
        {
          title: "Tipo",
          key: "type",
        },
        {
          title: "Valor",
          key: "value",
        },
        {
          title: "Quantidade",
          key: "use",
        },
        {
          title: "action",
          key: "action",
          sortable: false,
        },
      ],
      items: [],
    };
  },

  async created() {
    await this.getItems();
  },

  computed: {
    tituloDialog: function () {
      return this.cupom.id ? "Editar" : "Criar";
    },
  },

  watch: {
    ativo(valor) {
      if (valor == false) {
        this.resetCupom();
      }
    },
  },

  methods: {
    resetCupom() {
      this.product = {
        nome: null,
        status: null,
      };
      this.ativo = false;
    },

    async persist() {
      if (this.cupom.id) {
        const response = await this.$api.patch(`/cupom/persist/${this.cupom.id}`, this.cupom);
      } else {
        const response = await this.$api.post("/cupom/persist", this.cupom);
      }
      this.resetCupom();
      await this.getItems();
    },

    editItem(item) {
      this.cupom = {
        ...item,
      };
      this.ativo = true;
    },

    async getItems() {
      const response = await this.$api.get("/cupom");
      this.items = response.response;
      return response;
    },

    async deleteItem(item) {
      if (confirm(`Deseja deletar?`)) {
        const response = await this.$api.delete(`/cupom/delete/${item.id}`);
      }
      if (response.type == "error") {
        alert(response.message);
      }
      this.resetCupom();
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
  