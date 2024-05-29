<template>
    <template style="background-color: bisque;">
      <v-card style="background-color: #f5ebdc;">
        <v-tabs
          v-model="tab"
          align-tabs="center"
          color="deep-purple-accent-4"
        >
            <v-tab :value="1" @click="" style="color: cadetblue; font-weight:
             bolder; font-size:larger;" class="text-none">Início</v-tab>
            <v-tab :value="2" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">Cupons</v-tab>
            <v-tab :value="3" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">Cardápio</v-tab>
            <v-tab :value="4" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">App BK</v-tab>
            <v-tab :value="5" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">Clube BK</v-tab>
            <v-tab :value="6" @click="" style="color: cadetblue; font-weight: bold; 
            font-size:large;" class="text-none">Delivery</v-tab>
        </v-tabs>
      </v-card>
    </template>

  </template>
  
  <script>
    export default {
      data: () => ({
        tab: null,
      }),
    }
  </script>
  
  <style>
  
  </style>