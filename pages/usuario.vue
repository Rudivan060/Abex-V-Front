  <template>
    <div>
      <v-app>
        <v-container>
          <TabelaComponent 
            titulo="Usuarios" 
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
            <v-card height="550" width="500" theme="dark">
              <v-card-title>
                Criar
              </v-card-title>
              <v-card-text>
                <v-row>
                  <v-col>
                    <v-text-field
                      v-model="usuario.cpf"
                      variant="outlined" 
                      placeholder="cpf" 
                      item-title="Cpf" 
                      item-value="cpf"
                    />
                  </v-col>
                  
                  <v-col>
                    <v-text-field 
                      v-model="usuario.nome"
                      placeholder="Nome" 
                      item-title="nome" 
                      item-value="nome"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="usuario.telefone"
                      placeholder="Insira seu telefone"
                      item-title="telefone" 
                      item-value="telefone"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="usuario.email"
                      placeholder="Email"
                      item-title="email" 
                      item-value="email"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-autocomplete 
                      v-model="usuario.status"
                      :items="status" 
                      placeholder="Presença"
                      item-title="label" 
                      item-value="status"
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
                      v-model="usuario.cpf"
                      variant="outlined" 
                      placeholder="cpf" 
                      item-title="Cpf" 
                      item-value="cpf"
                    />
                  </v-col>
                  
                  <v-col>
                    <v-text-field 
                      v-model="usuario.nome"
                      placeholder="Nome" 
                      item-title="nome" 
                      item-value="nome"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="usuario.telefone"
                      placeholder="Insira seu telefone"
                      item-title="telefone" 
                      item-value="telefone"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field 
                      v-model="usuario.email"
                      placeholder="Email"
                      item-title="email" 
                      item-value="email"
                    />
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-autocomplete 
                      v-model="usuario.status"
                      :items="status" 
                      placeholder="Presença"
                      item-title="label" 
                      item-value="status"
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
          usuario: {
            cpf: null,
            nome: null,
            telefone: null,
            email: null,
            status: false,
          },
          headers: [
            {
              title: "Nomes",
              key: "nome",
            },
            {
              title: "telefone",
              key: "telefone",
            },
            {
              title: "",
              key: "action",
              sortable: false,
            },
          ],
          items: [],
          status: [
            { status: true, label: 'Ativo' },
            { status: false, label: 'Inativo' }
          ],
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
          this.usuario = {
            cpf: null,
            nome: null,
            telefone: null,
            email: null,
            status: false,
          };
          this.ativo = false;
          this.ativo2 = false;
        },

        async getItems() {
          this.loading = true;
          try {
            const response = await this.$api.get("/usuario");
            this.items = response.response;
          } catch (error) {
            console.error("Erro ao carregar itens:", error);
          } finally {
            this.loading = false;
            console.log("dados carregados");
          }
        },

        editItem(item) {
          this.usuario = {
            ...item,
          };
          this.ativo2 = true;
        },

        async create() {
          const response = await this.$api.post("/usuario/create", this.usuario);
          console.log("Criando item");
          await this.getItems();
          this.resetUsuario();
        },

        async edit() {
          const response = await this.$api.patch(`/usuario/update/${this.usuario.cpf}`, this.usuario);
          console.log("Editando item");
          await this.getItems();
          this.resetUsuario();
        },

        async deleteItem(item) {  
          if (confirm(`Deseja deletar o registro com cpf ${item.cpf}`)) {
            this.loading = true;
            try {
              const response = await this.$api.delete(`/usuario/delete/${item.cpf}`);
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
