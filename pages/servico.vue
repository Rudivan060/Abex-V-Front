<template>
    <div>
      <v-app>
        <v-container>
          <TabelaComponent 
            titulo="Serviços" 
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
            <v-card height="550" width="550" theme="dark">
              <v-card-title>
                Criar
              </v-card-title>
              <v-card-text>
                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="servico.nome"
                      placeholder="Tipo de Serviço" 
                      item-title="data" 
                      item-value="data"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-textarea 
                      v-model="servico.descricaoServico"
                      placeholder="Descricao do Serviço" 
                      item-title="descricaoServico" 
                      item-value="descricaoServico"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="servico.valor"
                      placeholder="Valor R$ " 
                      item-title="valor" 
                      item-value="valor"
                    />
                  </v-col>

                  <v-col>
                    <v-autocomplete 
                      v-model="servico.idPedido"
                      :items="pedido" 
                      placeholder="Id do Pedido"
                      item-title="label" 
                      item-value="id"
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
                      v-model="servico.nome"
                      placeholder="Tipo de Serviço" 
                      item-title="data" 
                      item-value="data"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-textarea 
                      v-model="servico.descricaoServico"
                      placeholder="Descricao do Serviço" 
                      item-title="descricaoServico" 
                      item-value="descricaoServico"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="servico.valor"
                      placeholder="Valor R$ " 
                      item-title="valor" 
                      item-value="valor"
                    />
                  </v-col>

                  <v-col>
                    <v-autocomplete 
                      v-model="servico.idPedido"
                      :items="pedido" 
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
          servico: {
            id: null, 
            nome: null, 
            descricaoServico: null, 
            valor: null, 
            idPedido: null,
          },
          headers: [
            {
              title: "Id",
              key: "id",
            },
            {
              title: "Tipo de Serviço",
              key: "nome",
            },
            {
              title: "Valor R$",
              key: "valor",
            },
            {
              title: "",
              key: "action",
              sortable: false,
            },
          ],
          items: [],
          pedido: [],
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
      },

      methods: {
        resetUsuario() {
          this.servico = {
            id: null, 
            nome: null, 
            descricaoServico: null, 
            valor: null, 
            idPedido: null,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/servico");
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

        editItem(item) {
          this.servico = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          const response = await this.$api.post("/servico/create", this.servico);
          console.log("Criando item");
          await this.getItems();
          this.resetUsuario();
        },

        async edit() {
          const response = await this.$api.patch(`/servico/update/${this.servico.id}`, this.servico);
          console.log("Editando item");
          await this.getItems();
          this.resetUsuario();
        },

        async deleteItem(item) {  
          if (confirm(`Deseja deletar o registro com cpf ${item.id}`)) {
            this.loading = true;
            try {
              const response = await this.$api.delete(`/servico/delete/${item.id}`);
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
