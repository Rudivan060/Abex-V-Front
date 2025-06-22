<template>
  <div style="min-height: 100vh;" class="degrade">
    <v-card style="background-color: #fb911f;">
      <v-tabs v-model="tab" align-tabs="center" color="red" class="custom-tabs-height">
          <a href="./">
            <v-tab
              :value="1" 
              class="text-none custom-tab" 
              style="color: darkred; font-weight: bolder; font-size: larger; margin-right: 70px;"
            >
              Início
            </v-tab>
          </a>  

          <a href="./cardapio">
            <v-tab
              :value="2" 
              class="text-none custom-tab" 
              style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
            >
              Cardápio
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

          <a href="./ademiro">
            <v-tab
              :value="4" 
              class="text-none custom-tab" 
              style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;"
            >
              Admin
            </v-tab>
          </a>
      </v-tabs>
    </v-card>

    <v-container 
      class="d-flex justify-center align-center" 
      fill-height 
    >
      <v-card width="400" elevation="8" class="pa-4">
        <v-card-title class="text-h5 text-center">Login</v-card-title>
        <v-card-text>
          <v-text-field
            v-model="cpf"
            label="CPF"
            type="password"
            prepend-inner-icon="mdi-lock"
            variant="outlined"
            density="comfortable"
            class="mt-4"
          />
          <v-btn color="primary" block class="mt-6" size="large" @click="handleLogin">
            Entrar
          </v-btn>
        </v-card-text>

        <v-card-actions class="justify-center">
          <v-btn variant="text" size="small" class="mt-2">Esqueceu a senha?</v-btn>
        </v-card-actions>
      </v-card>
    </v-container>
  </div>
</template>

<script>
  export default {
    data: () => ({
      tab: 4,
      items: [],
      cpf: '',
      loading: false,
    }),
    
    methods: {
      async handleLogin() {
        this.loading = true;
        try {
          const response = await this.$api.get("/administrador");
          this.items = response.response;
          const found = this.items.find(admin => admin.cpf === this.cpf);
          if (found) {
            alert("CPF encontrado! Bem-vindo, " + found.nome);
            window.location.href = "./admin/administrador";
          } else {
            alert("CPF não encontrado!");
          }
        } catch (error) {
          console.error("Erro ao carregar itens:", error);
        } finally {
          this.loading = false;
          console.log("dados carregados");
        }
      },
    }
  }
</script>

<style>
  html, body, #__nuxt, #__layout, .page-wrapper {
    height: 100%;
    min-height: 100vh;
  }

  .v-application--wrap {
    min-height: 100vh;
  }

  .v-container {
    min-height: 100vh;
  }

  .degrade {
    background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
    background-repeat: no-repeat;
    background-size: cover;
    height: 100vh;
    z-index: 0;
  }
</style>