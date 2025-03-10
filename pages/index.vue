<template>
  <v-app>
    <div class="degrade">
      <v-card style="background-color:  #fb911f;">
        <v-tabs v-model="tab" align-tabs="center" color="red" class="custom-tabs-height">
          <a href="./">
            <v-tab
            :value="1" class="text-none custom-tab" style="color: darkred; font-weight: bolder; font-size: larger; margin-right: 70px;">Início</v-tab>
          </a>  
    
          <a href="./cardapio">
            <v-tab
            :value="2" class="text-none custom-tab" style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;">Cardápio</v-tab>
          </a>
    
          <a href="./comanda">
            <v-tab
            :value="3" class="text-none custom-tab" style="color: darkred; font-weight: bold; font-size: large; margin-right: 70px;">Comanda</v-tab>
          </a>
          
          <a href="http://localhost:3000/login">
            <v-icon class="signOut" style="margin-right: 50px;">
              mdi-login
            </v-icon>
          </a>
        </v-tabs>
      </v-card>
    </div>
  </v-app>
</template>
  
<script>
  export default {
    data: () => ({
      tab: 1,
    }),
  }
</script>

<style>
  .custom-tabs-height {
    height: 70px !important;
    display: flex;
    align-items: center;
  }

  .custom-tab {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
  }

  .signOut {
    color: darkred;
  }

  .degrade {
    background: rgb(2, 0, 36);
    background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
    background-repeat: no-repeat;
    height: 100vh;
  }
</style>