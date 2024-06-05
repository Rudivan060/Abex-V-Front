<template>
  <template>
    <v-card style="background-color: orangered;">
      <v-tabs v-model="tab" align-tabs="center" color="deep-purple-accent-4">

        <a href="http://localhost:3000/">
          <v-tab :value="1" @click="" style="color: cadetblue; font-weight: bolder; 
          font-size:larger;" class="text-none">Início</v-tab>
        </a>

        <a href="">
          <v-tab :value="2" @click="" style="color: cadetblue; font-weight: bold; 
          font-size:large;" class="text-none">Cupons</v-tab>
        </a>

        <a href="http://localhost:3000/cardapio">
          <v-tab :value="3" @click="" style="color: cadetblue; font-weight: bold;
          font-size:large;" class="text-none">Cardápio</v-tab>
        </a>

        <a href="">
          <v-tab :value="4" @click="" style="color: cadetblue; font-weight: bold;
          font-size:large;" class="text-none">AppBK</v-tab>
        </a>

        <a href="">
          <v-tab :value="5" @click="" style="color: cadetblue; font-weight: bold;
          font-size:large;" class="text-none">ClubeBK</v-tab>
        </a>

        <a href="">
          <v-tab :value="6" @click="" style="color: cadetblue; font-weight: bold; 
          font-size:large;" class="text-none">Delivery</v-tab>
        </a>

      </v-tabs>
    </v-card>
  </template>

  <body class="degrade">
    <v-row>
      <v-col>

      </v-col>
      <v-col>


      </v-col>
    </v-row>
  </body>

</template>

<script>
  export default {
    data: () => ({
      tab: null,
    }),
  }
</script>

<style>
    .intro {
      display: table;
      width: 100%;
      height: 100%;
      padding: 100px 0;
      text-align: center;
      color: white;
      background: url(../img/Banner-Site.png) no-repeat bottom center scroll;
      background-position: 30% 45%;
      background-color: black;
      -webkit-background-size: cover;
      -moz-background-size: cover;
      background-size: cover;
      -o-background-size: cover;
    }

    .degrade {
      background: rgb(2,0,36);
      background: linear-gradient(0deg, rgba(2,0,36,1) 0%, rgba(182,52,25,1) 0%, rgba(255,190,0,1) 100%); 
    }
</style>