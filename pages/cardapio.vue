<template>
  <v-card style="background-color: #f5ebdc;">
    <v-tabs v-model="tab" align-tabs="center" color="deep-purple-accent-4">
      <v-tab :value="1" @click="" style="color: cadetblue; font-weight: bolder; font-size:larger;"
        href="http://localhost:3000/" class="text-none">Início</v-tab>
      <v-tab :value="2" @click="" style="color: cadetblue; font-weight: bold; font-size:large;"
        class="text-none">Cupons</v-tab>
      <v-tab :value="3" @click="" style="color: cadetblue; font-weight: bold; font-size:large;"
        class="text-none" href="http://localhost:3000/cardapio">Cardápio</v-tab>
      <v-tab :value="4" @click="" style="color: cadetblue; font-weight: bold; font-size:large;" class="text-none">App
        BK</v-tab>
      <v-tab :value="5" @click="" style="color: cadetblue; font-weight: bold; font-size:large;" class="text-none">Clube
        BK</v-tab>
      <v-tab :value="6" @click="" style="color: cadetblue; font-weight: bold; font-size:large;"
        class="text-none">Delivery</v-tab>
    </v-tabs>
  </v-card>

  <v-container>
    <v-row >
      <v-col v-for="(item, i) in items" :key="i" cols="12" md="4">
        <v-card class="mx-auto" max-width="344">
          <v-img height="100%" :src="item.image" cover></v-img>
          <v-card-item>
            <div>
              <div class="text-h6 mb-1">{{ item.name }}</div>
            </div>
          </v-card-item>

          <v-card-actions>
            <v-btn color="orange-lighten-2" text>Encomendar</v-btn>
            <v-spacer></v-spacer>
            <v-btn 
            :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'"
            @click="show = !show"
            ></v-btn>
          </v-card-actions>

          <v-expand-transition>
            <div v-show="show">
              <v-divider></v-divider>
              <v-card-text>
                R$ {{ item.price }}
              </v-card-text>
              <v-card-text>
                Ingredientes: {{ item.description }}
              </v-card-text>
            </div>
          </v-expand-transition>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<!-- () => ativo = true" -->

<script>
export default {
  data: () => ({
    tab: 3,
    show: false,
    items: [],
  }),

  async created() {
    await this.getItems();
  },

  methods: {
    async getItems() {
      const response = await this.$api.get("/product");
      this.items = response.response;
      return response;
    },
  }
}
</script>

<style scoped>
.text-none {
  text-decoration: none;
}
</style>
