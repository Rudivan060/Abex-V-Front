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


  <v-app>
    <v-container style="border-radius: 6%;">
      <TabelaComponent 
      titulo="Tabelasso" 
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
                <v-text-field variant="outlined" label="Código" placeholder="Código" v-model="address.zipCode">
                </v-text-field>
              </v-col>

              <v-col>
                <v-text-field variant="outlined" label="Estado" placeholder="Estado" v-model="address.state">
                </v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <v-text-field variant="outlined" label="Cidade" placeholder="Cidade" v-model="address.city">
                </v-text-field>
              </v-col>

              <v-col>
                <v-text-field variant="outlined" label="Estada" placeholder="Estada" v-model="address.street">
                </v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <v-text-field variant="outlined" label="Cidade" placeholder="Cidade" v-model="address.district">
                </v-text-field>
              </v-col>

              <v-col>
                <v-text-field variant="outlined" label="Estada" placeholder="Estada" v-model="address.numberForget">
                </v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col>
                <v-text-field variant="outlined" label="Estada" placeholder="Estada" v-model="address.idUser">
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
          <v-list-item href="http://localhost:3000/crudaddress" prepend-icon="mdi-ticket-percent-outline"
            title="Cardápio" value="myfiles"></v-list-item>
          <v-list-item prepend-icon="mdi-account-multiple" title="Shared with me" value="shared"></v-list-item>
          <v-list-item prepend-icon="mdi-star" title="Starred" value="starred"></v-list-item>
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
        address: {
          zipCode: null,
          state: null,
          city: null,
          street: null,
          district: null,
          numberForget: null,
          idUser: null,
        },
        tab: null,
        search: "",
        headers: [
          {
            title: "Identificador",
            key: "id",
          },
          {
            title: "Cidade",
            key: "city",
          },
          {
            title: "Estado",
            key: "state",
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
        return this.address.id ? "Editar" : "Criar";
      },
    },
  
    watch: {
      ativo(valor) {
        if (valor == false) {
          this.resetaddress();
        }
      },
    },
  
    methods: {
      resetaddress() {
        this.product = {
          nome: null,
          status: null,
        };
        this.ativo = false;
      },
  
      async persist() {
        if (this.address.id) {
          const response = await this.$api.patch(`/address/persist/${this.address.id}`, this.address);
        } else {
          const response = await this.$api.post("/address/persist", this.address);
        }
        this.resetaddress();
        await this.getItems();
      },
  
      editItem(item) {
        this.address = {
          ...item,
        };
        this.ativo = true;
      },
  
      async getItems() {
        const response = await this.$api.get("/address");
        this.items = response.response;
        return response;
      },

      async deleteItem(item) {
        if (confirm(`Deseja deletar?`)) {
          const response = await this.$api.delete(`/address/delete/${item.id}`);
        }
        if (response.type == "error") {
          alert(response.message);
        }
        this.resetaddress();
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
    