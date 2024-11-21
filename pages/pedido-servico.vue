<template>
    <div>
      <v-app>
        <v-container>
          <TabelaComponent 
            titulo="Comandas" 
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
            <v-card height="250" width="500" theme="dark">
              <v-card-title>
                Criar
              </v-card-title>
              <v-card-text>
                <v-row>
                  <v-col>
                    <v-autocomplete 
                      v-model="pedidoServico.idServico"
                      :items="servico" 
                      placeholder="Id do Serviço"
                      item-title="label" 
                      item-value="idProduto"
                    />
                  </v-col>

                  <v-col>
                    <v-autocomplete 
                      v-model="pedidoServico.idPedido"
                      :items="pedido" 
                      placeholder="Id do Pedido"
                      item-title="label" 
                      item-value="idPedido"
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
              height="250" 
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
                      v-model="pedidoServico.idServico"
                      :items="servico" 
                      placeholder="Id do Serviço"
                      item-title="label" 
                      item-value="idProduto"
                    />
                  </v-col>

                  <v-col>
                    <v-autocomplete 
                      v-model="pedidoServico.idPedido"
                      :items="pedido" 
                      placeholder="Id do Pedido"
                      item-title="label" 
                      item-value="idPedido"
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
          pedidoServico: {
            idServico: null,
            idPedido: null,
          },
          headers: [
            {
              title: "Id do servico",
              key: "idServico",
            },
            {
              title: "Id do Pedido",
              key: "idPedido",
            },
            {
              title: "Quantidade",
              key: "quantidade",
            },
            {
              title: "",
              key: "action",
              sortable: false,
            },
          ],
          items: [],
          pedido: [],
          servico: [],
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
        await this.getIdServico();
      },

      methods: {
        resetUsuario() {
          this.pedidoServico = {
            idProduto: null,
            idPedido: null,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/pedido-servico");
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

        async getIdServico() {
          this.loading = true;
          try {
            const response = await this.$api.get(`/servico`);            
            this.servico = response.response;
            this.servico = this.servico.map((item) => {
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
          this.pedidoServico = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          if (!this.pedidoServico.idServico || !this.pedidoServico.idPedido) {
            alert("Por favor, preencha todos os campos antes de criar.");
            return;
          }
          
          const response = await this.$api.post("/pedido-servico/create", this.pedidoServico);
          console.log("Criando item");
          await this.getItems();
          this.resetUsuario();
        },

        async edit() {
          try {
            const response = await this.$api.patch(`/pedido-servico/update/${this.pedidoServico.idServico}/${this.pedidoServico.idPedido}`, this.pedidoServico);
            console.log("Editando item");
            await this.getItems();
            this.resetUsuario();
          } catch (error) {
            console.error("Erro ao editar item:", error);
          }
        },

        async deleteItem(item) {  
          if (confirm(`Deseja deletar o registro com ids ${item.idProduto} e  ${item.idPedido}`)) {
            this.loading = true;
            try {
              const response = await this.$api.delete(`/pedido-servico/delete/${item.idServico}/${item.idPedido}`, this.pedidoServico);
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
