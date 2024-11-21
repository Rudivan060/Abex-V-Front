<template>
    <div>
      <v-app>
        <v-container>
          <TabelaComponent 
            titulo="Histórico Compras" 
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
                    <v-autocomplete 
                      v-model="historicoCompras.idConsumidor"
                      :items="consumidor" 
                      placeholder="Id do Serviço"
                      item-title="label" 
                      item-value="idConsumidor"
                    />
                  </v-col>

                  <v-col>
                    <v-autocomplete 
                      v-model="historicoCompras.idPedido"
                      :items="pedido" 
                      placeholder="Id do Pedido"
                      item-title="label" 
                      item-value="idPedido"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="historicoCompras.data"
                      placeholder="Data"
                      item-title="data" 
                      item-value="data"
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
                    <v-autocomplete 
                      v-model="historicoCompras.idConsumidor"
                      :items="consumidor" 
                      placeholder="Id do Serviço"
                      item-title="label" 
                      item-value="idConsumidor"
                    />
                  </v-col>

                  <v-col>
                    <v-autocomplete 
                      v-model="historicoCompras.idPedido"
                      :items="pedido" 
                      placeholder="Id do Pedido"
                      item-title="label" 
                      item-value="idPedido"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="historicoCompras.data"
                      placeholder="Data"
                      item-title="data" 
                      item-value="data"
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
          historicoCompras: {
            data: null,
            idConsumidor: null,
            idPedido: null,
          },
          headers: [
            {
              title: "Id",
              key: "id",
            },
            {
              title: "Data",
              key: "data",
            },
            {
              title: "Id do Pedido",
              key: "idPedido",
            },
            {
              title: "Id do Consumidor",
              key: "idConsumidor",
            },
            {
              title: "",
              key: "action",
              sortable: false,
            },
          ],
          items: [],
          pedido: [],
          consumidor: [],
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
        await this.getIdPedido();
        await this.getIdConsumidor();
      },

      methods: {
        resetUsuario() {
          this.historicoCompras = {
            data: null,
            idConsumidor: null,
            idPedido: null,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/historico-compras");
            this.items = response.response;
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            this.loading = false;
            console.log("dados carregados");
          }
        },

        async getIdPedido() {
          this.loading = true;
          try {
            const response = await this.$api.get(`/pedido`);
            this.pedido = response.response;
            this.pedido = this.pedido.map((item) => {
              return {
                label: item.id,
                id: item.id,
              };
            });            
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            console.log("pedido carregados");
          }
        },

        async getIdConsumidor() {
          this.loading = true;
          try {
            const response = await this.$api.get(`/consumidor`);            
            this.consumidor = response.response;
            this.consumidor = this.consumidor.map((item) => {
              return {
                label: item.id,
                id: item.id,
              };
            });            
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            console.log("pedido carregados");
          }
        },

        editItem(item) {
          this.historicoCompras = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          if (!this.historicoCompras.idConsumidor || !this.historicoCompras.idPedido || !this.historicoCompras.data) {
            alert("Por favor, preencha todos os campos antes de criar.");
            return;
          }

          try {
            const response = await this.$api.post("/historico-compras/create", this.historicoCompras);
            console.log("Item criado com sucesso", response);
            await this.getItems();
            this.resetUsuario();
          } catch (error) {
            console.error("Erro ao criar item:", error);
          }
        },

        async edit() {
          try {
            const response = await this.$api.patch(`/historico-compras/update/${this.historicoCompras.id}`, this.historicoCompras);
            console.log("Editando item");
            await this.getItems();
            this.resetUsuario();
          } catch (error) {
            console.error("Erro ao editar item:", error);
          }
        },

        async deleteItem(item) {  
          if (confirm(`Deseja deletar o registro com ids ${item.id}`)) {
            this.loading = true;
            try {
              const response = await this.$api.delete(`/historico-compras/delete/${item.id}`);
              console.log("Deletando item");
              await this.getItems();
              this.loading = false;
            } catch (error) {
              console.error("Erro ao excluir item:", error);
            }
          }
        }
      },
    };
  </script>
