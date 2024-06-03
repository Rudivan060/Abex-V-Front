<template>
    <v-card style="background-color: #f5ebdc;">
      <v-tabs v-model="tab" align-tabs="center" color="deep-purple-accent-4">
        <v-tab :value="1" @click="" style="color: cadetblue; font-weight: bolder; font-size:larger;" href="http://localhost:3000/" class="text-none">Início</v-tab>
        <v-tab :value="2" @click="" style="color: cadetblue; font-weight: bold; font-size:large;" class="text-none">Cupons</v-tab>
        <v-tab :value="3" @click="" style="color: cadetblue; font-weight: bold; font-size:large;" class="text-none">Cardápio</v-tab>
        <v-tab :value="4" @click="" style="color: cadetblue; font-weight: bold; font-size:large;" class="text-none">App BK</v-tab>
        <v-tab :value="5" @click="" style="color: cadetblue; font-weight: bold; font-size:large;" class="text-none">Clube BK</v-tab>
        <v-tab :value="6" @click="" style="color: cadetblue; font-weight: bold; font-size:large;" class="text-none">Delivery</v-tab>
      </v-tabs>
    </v-card>
    
    <!-- v-for="(item, i) in items" :key="i" cols="12" md="6" -->

    <v-container>
      <v-row>
        <v-col 
        >
          <v-card class="mx-auto" max-width="344">
            <v-img height="200px" src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg" cover></v-img>

            <v-card-item>
              <div>
                <div class="text-overline mb-1">{{ item }}</div>
                <div class="text-h6 mb-1">Headline</div>
                <div class="text-caption">Greyhound divisely hello coldly fonwderfully</div>
              </div>
            </v-card-item>

            <v-card-actions>
              <v-btn color="orange-lighten-2" text>Explore</v-btn>
              <v-spacer></v-spacer>
              <v-btn :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'" @click="show = !show"></v-btn>
            </v-card-actions>

            <v-expand-transition>
              <div v-show="show">
                <v-divider></v-divider>
                <v-card-text>
                  I'm a thing. But, like most politicians, he promised more than he could deliver. You won't have time for sleeping, soldier, not with all the bed making you'll be doing. Then we'll go with that data file! Hey, you add a one and two zeros to that or we walk! You're going to do his laundry? I've got to find a way to escape.
                </v-card-text>
              </div>
            </v-expand-transition>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
</template>

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
      try {
        const response = await this.$api.get("/product");
        this.items = response.data;  // Assuming 'response.data' holds the desired items array
      } catch (error) {
        console.error("Failed to fetch items:", error);
      }
    }
  }
}
</script>

<style scoped>
.text-none {
  text-decoration: none;
}
</style>
