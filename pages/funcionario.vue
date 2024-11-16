<template>
    <div>
      <v-app>
        <v-container>
          <TabelaComponent 
            titulo="Consumidores" 
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
            <v-card height="350" width="500" theme="dark">
              <v-card-title>
                Criar
              </v-card-title>
              <v-card-text>
                <v-row>
                  
                  <v-col>
                    <v-text-field 
                      v-model="funcionario.funcao"
                      placeholder="Função" 
                      item-title="funcao" 
                      item-value="funcao"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-autocomplete 
                      v-model="funcionario.cpfUsuario"
                      :items="cpfs" 
                      placeholder="cpf"
                      item-title="cpf" 
                      item-value="cpfUsuario"
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
                      v-model="funcionario.funcao"
                      placeholder="Função" 
                      item-title="funcao" 
                      item-value="funcao"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-autocomplete 
                      v-model="funcionario.cpfUsuario"
                      :items="cpfs" 
                      placeholder="Presença"
                      item-title="cpf" 
                      item-value="cpf"
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
          funcionario: {
            id: null,
            funcao: null,
            cpfUsuario: null,
          },
          headers: [
            {
              title: "Id",
              key: "id",
            },
            {
              title: "Função",
              key: "funcao",
            },
            {
              title: "",
              key: "action",
              sortable: false,
            },
          ],
          items: [],
          cpfs: [],
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
        await this.getCpf();
      },

      methods: {
        resetUsuario() {
          this.funcionario = {
            id: null,
            funcao: null,
            cpfUsuario: null,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/funcionario");
            this.items = response.response;
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            this.loading = false;
            console.log("dados carregados");
          }
        },

        async getCpf() {
          this.loading = true;
          try {
            const response = await this.$api.get(`/usuario`);            
            this.cpfs = response.response;
            this.cpfs = this.cpfs.map((item) => {
              return {
                label: item.cpf,
                cpf: item.cpf,
              };
            });            
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            console.log("cpfs carregados");
          }
        },

        editItem(item) {
          this.funcionario = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          const response = await this.$api.post("/funcionario/create", this.funcionario);
          console.log("Criando item");
          await this.getItems();
          this.resetUsuario();
        },

        async edit() {
          const response = await this.$api.patch(`/funcionario/update/${this.funcionario.id}`, this.funcionario);
          console.log("Editando item");
          await this.getItems();
          this.resetUsuario();
        },

        async deleteItem(item) {  
          if (confirm(`Deseja deletar o registro com cpf ${item.id}`)) {
            this.loading = true;
            try {
              const response = await this.$api.delete(`/funcionario/delete/${item.id}`);
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
