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
        class="mt-13"
        height="575"
        titulo="Produtos" 
        :items="items" 
        :headers="headers" 
        @deletou="deleteItem"
        @abrir-dialog="create()" 
      />
    </v-container>
  </div>
</template>

<script>
  export default {
    data: () => {
      return {
        valor: 0,
        tab: 6,
        loading: true,
        search: "",
        token: {
          id: null, 
        },
        headers: [
          {
            title: "Token",
            key: "id",
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

    async created() {
      await this.getItems();
    },

    methods: {
      async getItems() {
        this.loading = true;
        try {
          const response = await this.$api.get("/token");
          this.items = response.response;
        } catch (error) {
          console.error("Erro ao carregar itens:", error);
        } finally {
          this.loading = false;
          console.log("dados carregados");
        }
      },

      async create() {
        const response = await this.$api.post("/token/create", this.token);
        console.log("Criando item");
        await this.getItems();
        this.resetUsuario();
      },

      async edit() {
        const response = await this.$api.patch(`/token/update/${this.token.id}`, this.token);
        console.log("Editando item");
        await this.getItems();
        this.resetUsuario();
      },

      async deleteItem(item) {  
        if (confirm(`Deseja deletar o registro com Id ${item.id}`)) {
          this.loading = true;
          try {
            const response = await this.$api.delete(`/token/delete/${item.id}`);
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
