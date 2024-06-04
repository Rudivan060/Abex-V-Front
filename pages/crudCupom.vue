<template>
  <template>
    <v-card style="background-color: orangered;">
      <v-tabs v-model="tab" align-tabs="center" color="deep-purple-accent-4">

        <a href="http://localhost:3000/">
          <v-tab :value="1" @click="" style="color: cadetblue; font-weight: bolder; 
          font-size:larger;" class="text-none">Início</v-tab>
        </a>

        <a href="">
          <v-tab :value="2" @click="" style="color: cadetblue; font-weight: bold; 
          font-size:large;" class="text-none">Cupons</v-tab>
        </a>

        <a href="http://localhost:3000/cardapio">
          <v-tab :value="3" @click="" style="color: cadetblue; font-weight: bold;
          font-size:large;" class="text-none">Cardápio</v-tab>
        </a>

        <a href="">
          <v-tab :value="4" @click="" style="color: cadetblue; font-weight: bold;
          font-size:large;" class="text-none">AppBK</v-tab>
        </a>

        <a href="">
          <v-tab :value="5" @click="" style="color: cadetblue; font-weight: bold;
          font-size:large;" class="text-none">ClubeBK</v-tab>
        </a>

        <a href="">
          <v-tab :value="6" @click="" style="color: cadetblue; font-weight: bold; 
          font-size:large;" class="text-none">Delivery</v-tab>
        </a>

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
                  <v-text-field 
                  variant="outlined" 
                  label="Código" 
                  placeholder="Código" 
                  v-model="cupom.code">
                  </v-text-field>
                </v-col>

                <v-col>
                  <v-text-field 
                  variant="outlined" 
                  label="Tipo" 
                  placeholder="Tipo" 
                  v-model="cupom.type">
                  </v-text-field>
                </v-col>
              </v-row>
  
              <v-row>
                <v-col>
                  <v-text-field 
                    variant="outlined" 
                    label="Valor" 
                    placeholder="Valor"
                    v-model="cupom.value">
                  </v-text-field>
                </v-col>

                <v-col>
                  <v-text-field 
                  variant="outlined" 
                  label="Quantidade" 
                  placeholder="Quantidade" 
                  v-model="cupom.use">
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
  