<template>
  <template>
    <v-card style="background-color:  #fb911f;">
      <v-tabs v-model="tab" align-tabs="center" color="red">
        <a href="http://localhost:3000/ademiro">
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


  <div class="degrade"> 
    <v-row>
      <v-col>
        <section>
          <img src="/imagem/burgasso.png" alt=""
            style="height: 68%; width: 40%; 
            position: absolute; top: 15%; left: 10%;">
        </section>
      </v-col>
      <v-col>
        <template>
          <div style="position: absolute; top: 15%; left: 55%;">
            <v-card class="mx-auto pa-12 pb-8" elevation="24" max-width="448" rounded="lg">
              <div class="text-subtitle-1 text-medium-emphasis">E-mail</div>
  
              <v-text-field density="compact" placeholder="Email address" prepend-inner-icon="mdi-email-outline"
                variant="outlined"></v-text-field>
  
              <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
                Senha
  
                <a class="text-caption text-decoration-none text-blue" href="#" rel="noopener noreferrer" target="_blank">
                  Forgot login password?</a>
              </div>
  
              <v-text-field :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'" :type="visible ? 'text' : 'password'"
                density="compact" placeholder="Enter your password" prepend-inner-icon="mdi-lock-outline"
                variant="outlined" @click:append-inner="visible = !visible"></v-text-field>
  
              <v-card class="mb-12" color="surface-variant" variant="tonal">
                <v-card-text class="text-medium-emphasis text-caption">
                  AVISO: Depois de 3 tentativas de login falhas, sua conta será bloqueada temporariamente por 3 horas. 
                  caso não queira esperar, você pode clicar em "Esqueci minha senha?" acima da senha de usuário.
                </v-card-text>
              </v-card>
  
              <v-btn class="mb-8" color="blue" size="large" variant="tonal" block>
                Log In
              </v-btn>
  
              <v-card-text class="text-center">
                <a class="text-blue text-decoration-none" href="http://localhost:3000/cadastro" rel="noopener noreferrer" target="_blank">
                  Sign up now <v-icon icon="mdi-chevron-right"></v-icon>
                </a>
              </v-card-text>
            </v-card>
          </div>
        </template>
      </v-col>
    </v-row>
  </div>


</template>

<script>
  export default {
    data: () => ({
      tab: null,
      visible: false,
    }),
  }
</script>

<style>
  .bah {
    color: darkred
  }

  .degrade {
    background: rgb(2, 0, 36);
    background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
    background-repeat: no-repeat;
    height: 100dvh;
  }
</style>



        