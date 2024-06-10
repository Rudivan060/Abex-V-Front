<template>
  <template>
    <v-card style="background-color:  #fb911f;">
      <v-tabs v-model="tab" align-tabs="center" color="red">
        <a href="http://localhost:3000/ademiro/ademiro">
          <v-icon class="bah">
            mdi-account-lock
          </v-icon>
        </a>

        <a href="http://localhost:3000/">
          <v-tab :value="1" @click="" style="color: darkred; font-weight: bolder; 
          font-size:larger;" class="text-none">Início</v-tab>
        </a>  

        <a href="https://www.youtube.com/watch?v=ycHVUvvOwzY">
          <v-tab :value="2" @click="" style="color: darkred; font-weight: bold; 
          font-size:large;" class="text-none">Cupons</v-tab>
        </a>

        <a href="http://localhost:3000/cardapio">
          <v-tab :value="3" @click="" style="color: darkred; font-weight: bold;
          font-size:large;" class="text-none">Cardápio</v-tab>
        </a>

        <a href="https://www.youtube.com/watch?v=hvL1339luv0">
          <v-tab :value="4" @click="" style="color: darkred; font-weight: bold;
          font-size:large;" class="text-none">AppBK</v-tab>
        </a>

        <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ">
          <v-tab :value="5" @click="" style="color: darkred; font-weight: bold;
          font-size:large;" class="text-none">ClubeBK</v-tab>
        </a>
        
        <a href="http://localhost:3000/login">
          <v-icon class="bah" style="position: relative; width: 50%;">
            mdi-login
          </v-icon>
        </a>

      </v-tabs>
    </v-card>
  </template>

  <body>
    <section>
      <div class="background-section">
        <video src="../Video/a.mp4" autoplay loop muted></video>
      </div>
      <div class="box">
        <div class="conteudo">
          <pre>
            O AMOR PODE ACABAR
            O BOM GOSTO NÃO!
          </pre>
          <img src="/imagem/burgasso.png" alt="" style="height: 75%; width: 75%;">
        </div>
      </div>
    </section>
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
  *{
    padding: 0;
    margin: 0;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    box-sizing: border-box;
  }

  body{
    width: 100%;
    min-height: 100vh;
  }
  section{
    width: 100%;
    height: 100vh;  
  }

  section div.background-section{
    width: 100%;
    height: 100%;
    position: absolute;
    z-index: 0;
  }

  section div.background-section video{
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }

  section div.box{
    width: 50%;
    height: 100%;
    position: relative;
    z-index: 1;
    background: linear-gradient(to right, blue(0,0,0,0.9), transparent);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2%;
  }

  section div.box div.conteudo{
    width: 80%;
  }

  section div.box img{
    width: 100%;
    min-width: 300px;
  }

  section div.box pre{
    font-family: 'Times New Roman', Times, serif;
    font-weight: bolder;
    color: #b59143;
    font-size: 24px;
    max-height: 32px;
  }

  section div.box v-tab{
    color: yellow;
  }

  .bah{
    color:darkred
  }
</style>
