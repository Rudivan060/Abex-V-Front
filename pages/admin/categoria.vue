<template>
    <div>
      <v-app>
        <v-container>
          <TabelaComponent 
            titulo="Categorias" 
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
            <v-card height="450" width="500" theme="dark">
              <v-card-title>
                Criar
              </v-card-title>
              <v-card-text>
                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="categoria.nome"
                      placeholder="Categoria"
                      item-title="label" 
                      item-value="categoria"
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
              height="550" 
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
                      v-model="categoria.nome"
                      placeholder="Id do Pedido"
                      item-title="label" 
                      item-value="id"
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
      </v-app>
    </div>
  </template>

  <script>
    export default {
      data: () => {
        return {
          valor: 0,
          ativo: false,
          ativo2: false,
          loading: true,
          textoUsuario: null,
          tab: null,
          search: "",
          categoria: {
            id: null,
            nome: null,
          },
          headers: [
            {   
              title: "Id",
              key: "id",
            },
            {
              title: "Categoria",
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
          this.categoria = {
            id: null,
            nome: null,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/categoria");
            this.items = response.response;
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            this.loading = false;
            console.log("dados carregados");
          }
        },

        editItem(item) {
          this.categoria = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          const response = await this.$api.post("/categoria/create", this.categoria);
          console.log("Criando item");
          await this.getItems();
          this.resetUsuario();
        },

        async edit() {
          const response = await this.$api.patch(`/categoria/update/${this.categoria.id}`, this.categoria);
          console.log("Editando item");
          await this.getItems();
          this.resetUsuario();
        },

        async deleteItem(item) {  
          if (confirm(`Deseja deletar o registro com cpf ${item.id}`)) {
            this.loading = true;
            try {
              const response = await this.$api.delete(`/categoria/delete/${item.id}`);
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
