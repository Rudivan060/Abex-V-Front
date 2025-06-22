<template>
  <div class="degrade">
    <v-card style="background-color: #fb911f;">
      <v-tabs v-model="tab" align-tabs="center" color="red" class="custom-tabs-height">
        <a href="./administrador">
          <v-tab
            :value="1" 
            class="text-none custom-tab" 
            style="color: darkred; font-weight: bolder; font-size: larger; margin-right: 70px;"
          >
            Administradores
          </v-tab>
        </a>  

        <a href="./categoria">
          <v-tab
            :value="2" 
            class="text-none custom-tab" 
            style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
          >
            Categorias
          </v-tab>
        </a>

        <a href="./comanda">
          <v-tab
            :value="3" 
            class="text-none custom-tab" 
            style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
          >
            Comanda
          </v-tab>
        </a>

        <a href="./pedido">
          <v-tab
            :value="4" 
            class="text-none custom-tab" 
            style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
          >
            Pedidos
          </v-tab>
        </a>

        <a href="./produto">
          <v-tab
            :value="5" 
            class="text-none custom-tab" 
            style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
          >
            Produtos
          </v-tab>
        </a>

        <a href="./token">
          <v-tab
            :value="6" 
            class="text-none custom-tab" 
            style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
          >
            Token
          </v-tab>
        </a>

        <a href="../">
          <v-tab
            :value="7" 
            class="text-none custom-tab" 
            style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
          >
            Home
          </v-tab>
        </a>
      </v-tabs>
    </v-card>

    <v-container>
      <TabelaComponent 
        titulo="Administradores"
        height="575"
        class="mt-13" 
        :items="items" 
        :headers="headers" 
        @editou="editItem" 
        @deletou="deleteItem"
        @abrir-dialog="() => ativo = true" 
        @dialog-edit="() => ativo = true"
      />
      <v-dialog 
        v-model="ativo"
        max-width="500"
      >
        <v-card 
          height="350" 
          width="500" 
          theme="dark"
        >
          <v-card-title>
            Criar
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col>
                <v-text-field 
                  v-model="administrador.cpf"
                  placeholder="CPF"
                  item-title="label" 
                  item-value="cpf"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field 
                  v-model="administrador.nome"
                  placeholder="Nome"
                  item-title="label" 
                  item-value="cpf"
                />
              </v-col>
            </v-row>
          </v-card-text>

          <v-card-actions>
            <v-btn variant="outlined" @click="create()">
              Criar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog 
        v-model="ativo2"
        max-width="500"
      >
        <v-card 
          height="350" 
          width="500" 
          theme="dark"
        >
          <v-card-title>
            Editar
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col>
                <v-text-field 
                  v-model="administrador.cpf"
                  placeholder="CPF"
                  item-title="label" 
                  item-value="cpf"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field 
                  v-model="administrador.nome"
                  placeholder="Nome"
                  item-title="label" 
                  item-value="nome"
                />
              </v-col>
            </v-row>
          </v-card-text>

          <v-card-actions>
            <v-btn variant="outlined" @click="edit()">
              Editar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </div>
</template>

  <script>
    export default {
      data: () => {
        return {
          tab: 1,
          valor: 0,
          ativo: false,
          ativo2: false,
          loading: true,
          textoUsuario: null,
          search: "",
          administrador: {
            cpf: null,
            nome: null,
          },
          headers: [
            {   
              title: "cpf",
              key: "cpf",
            },
            {
              title: "nome",
              key: "nome",
            },
            {
              title: "",
              key: "action",  
              sortable: false,
            },
          ],
          items: [],
        };
      },

      watch: {
        ativo(valor) {
          if (valor == false) {
            this.resetUsuario();
          }
        },
        ativo2(valor) {
          if (valor == false) {
            this.resetUsuario();
          }
        },
        resetTabela() {
          if (this.loading == false) {
            this.getItems();
          }
        }
      },

      async created() {
        await this.getItems();
      },

      methods: {
        resetUsuario() {
          this.administrador = {
            cpf: null,
            nome: null,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/administrador");
            this.items = response.response;
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            this.loading = false;
            console.log("dados carregados");
          }
        },

        editItem(item) {
          this.administrador = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          const response = await this.$api.post("/administrador/create", this.administrador);
          console.log("Criando item");
          await this.getItems();
          this.resetUsuario();
        },

        async edit() {
          const response = await this.$api.patch(`/administrador/update/${this.administrador.cpf}`, this.administrador);
          console.log("Editando item");
          await this.getItems();
          this.resetUsuario();
        },

        async deleteItem(item) {
          if (confirm(`Deseja deletar o registro com cpf ${item.cpf}`)) {
            this.loading = true;
            try {
              const response = await this.$api.delete(`/administrador/delete/${item.cpf}`);
            } catch (error) {
              console.error("Erro ao excluir item:", error);
            }
            console.log("Deletando item");
            await this.getItems();
            this.loading = false;
          }
        }
      },
    };
  </script>

<style>
  .degrade {
    background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
    background-repeat: no-repeat;
    background-size: cover;
    height: 100vh;
    z-index: 0;
  }
</style>
