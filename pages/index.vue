<template>
  <div>
    <!-- <template v-if="loading || !gifLoaded">
      <div class="loading-container">
        <v-progress-circular
          indeterminate
          color="orange"
          size="100"
          width="10"
          class="mx-auto mt-10"
        />
      </div>
    </template> -->

    <v-card style="background-color: #fb911f;">
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
            mdi-account
          </v-icon>
        </a>
      </v-tabs>
    </v-card>

    <section>
      <div class="background-section">
        <img 
          src="../img/Piranha-_video-converter.com_.gif"  
          alt="Ta quentinho ta quentinho" 
          style="display: block;"
          @load="gifLoaded = true">
      </div>

      <div class="logo-container">
        <img 
          src="/img/Batata.png" 
          alt="Logo Bruguer" 
          style="display: block;"
          @load="gifLoaded = true"
        >
      </div>
    </section>
  </div>
</template>
  
<script>
export default {
  data: () => ({
    tab: 1,
    loading: false,
    gifLoaded: false,
  }),
}
</script>

<style>
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  }

  html, body {
    width: 100%;
    height: 100%;
    overflow: hidden; /* Evita scroll */
  }

  .custom-tab {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
  }

  .custom-tabs-height {
    height: 70px !important;
    display: flex;
    align-items: center;
  }

  section {
    width: 100vw;
    height: 100vh;
    position: relative;
    overflow: hidden;
  }

  section .background-section {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 0;
  }

  .loading-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  section .background-section img {
    width: 100%;
    height: 100vh;
    object-fit: cover;
    object-position: center;
  }

  section .logo-container {
    position: absolute;
    top: 8%;
    left: 4%;
    z-index: 2;
  }

  section .logo-container img {
    width: 150%;
    height: auto;
  }
</style>