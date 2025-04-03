<template>
    <div>
      <v-app>
        <v-container>
          <TabelaComponent 
            titulo="Produtos" 
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
            style="display: flex; justify-content: center; align-items: center; margin: 0 auto;"
          >
            <v-card height="625" width="650" theme="dark">
              <v-card-title>
                Criar
              </v-card-title>
              <v-card-text>
                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="produto.nome"
                      placeholder="Nome do produto" 
                      item-title="nome" 
                      item-value="nome"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-textarea 
                      v-model="produto.descricaoProduto"
                      placeholder="Descricao do produto" 
                      item-title="descricaoProduto" 
                      item-value="descricaoProduto"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="produto.quantidade"
                      placeholder="Quantidade" 
                      item-title="quantidade" 
                      item-value="quantidade"
                    />
                  </v-col>

                  <v-col>
                    <v-text-field 
                      v-model="produto.valor"
                      placeholder="Valor R$" 
                      item-title="valor" 
                      item-value="valor"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <img :src="produto.imagem" style="max-width: 100px; max-height: 100px">
                  </v-col>
                  <v-col class="justify-center align-center text-center" cols="8" sm="6" style="justify-content: center;">
                    <v-text-field v-model="produto.imagem" label="Link da imagem" />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-autocomplete 
                      v-model="produto.idPedido"
                      :items="pedido" 
                      placeholder="pedido"
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
              height="625" 
              width="650" 
              theme="dark"
            >
              <v-card-title>
                Editar
              </v-card-title>
              <v-card-text>
                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="produto.nome"
                      placeholder="Nome do produto" 
                      item-title="nome" 
                      item-value="nome"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-textarea 
                      v-model="produto.descricaoProduto"
                      placeholder="Descricao do produto" 
                      item-title="descricaoProduto" 
                      item-value="descricaoProduto"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="produto.quantidade"
                      placeholder="Quantidade" 
                      item-title="quantidade" 
                      item-value="quantidade"
                    />
                  </v-col>

                  <v-col>
                    <v-text-field 
                      v-model="produto.valor"
                      placeholder="Valor R$" 
                      item-title="valor" 
                      item-value="valor"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <img :src="produto.imagem" style="max-width: 100px; max-height: 100px">
                  </v-col>
                  <v-col class="justify-center align-center text-center" cols="8" sm="6" style="justify-content: center;">
                    <v-text-field v-model="produto.imagem" label="Link da imagem" />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-autocomplete 
                      v-model="produto.idPedido"
                      :items="pedido" 
                      placeholder="pedido"
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
          produto: {
            id: null, 
            nome: null, 
            descricaoProduto: null, 
            valor: null,
            imagem: null, 
            idPedido: null,
          },
          headers: [
            {
              title: "Id",
              key: "id",
            },
            {
              title: "Nome",
              key: "nome",
            },
            {
              title: "Quantidade Requisitada",
              key: "quantidade",
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
        await this.getIdPedido();
      },

      methods: {
        resetUsuario() {
          this.produto = {
            id: null, 
            nome: null, 
            descricaoProduto: null, 
            quantidade: null, 
            valor: null,
            imagem: null, 
            idPedido: null,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/produto");
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
          this.produto = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          const response = await this.$api.post("/produto/create", this.produto);
          console.log("Criando item");
          await this.getItems();
          this.resetUsuario();
        },

        async edit() {
          const response = await this.$api.patch(`/produto/update/${this.produto.id}`, this.produto);
          console.log("Editando item");
          await this.getItems();
          this.resetUsuario();
        },

        async deleteItem(item) {  
          if (confirm(`Deseja deletar o registro com Id ${item.id}`)) { 
            this.loading = true;
            try {
              const response = await this.$api.delete(`/produto/delete/${item.id}`);
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
