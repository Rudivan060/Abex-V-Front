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

        <a href="http://localhost:3000/cupom">
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
    <v-card class="mx-auto" style="height: 525px;
    width: 600px;
    position: absolute;
    top: 20%;
    left: 30%;
    border-radius: 16px;
    border: 5px solid darkred;
    ">

      <v-form>
        <v-text-field v-model="user.username" :rules="rules" label="Usuario" style="color: whitesmoke;"></v-text-field>

        <v-text-field v-model="user.cpf" :rules="rules" label="CPF" style="color: whitesmoke;"></v-text-field>

        <v-text-field v-model="user.name" :rules="rules" label="Nome" style="color: whitesmoke;"></v-text-field>

        <v-text-field v-model="user.phone" :rules="rules" label="Telefone" style="color: whitesmoke;"></v-text-field>

        <v-text-field v-model="user.password" :rules="rules" label="Senha" style="color: whitesmoke;"></v-text-field>

        <v-text-field v-model="user.email" :rules="rules" label="E-mail" style="color: whitesmoke;"></v-text-field>

      </v-form>

      <template v-slot:actions>
        <v-btn 
        append-icon="mdi-chevron-right" 
        color="yellow" 
        text="Cadastrar" 
        variant="outlined" 
        block position:
        absolute;
        @click="registerUser"
        ></v-btn>

      </template>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    tab: null,
    visible: false,
    user : {
      username: null,
      cpf: null,
      name: null,
      phone: null,
      password: null,
      email: null,
    },
    rules: [
      value => {
        if (value) return true

        return 'O campo não deve Ficar vazio!'
      },
    ],

    items: [],
  }),

  methods: {
    async registerUser() {
        const response = await this.$api.post('/user/register', this.user);
        this.resetUser();
      },
    
    resetUser() {
      this.user = {
        username: null,
        cpf: null,
        name:null,
        phone: null,
        password: null,
        email: null,
      };
      this.ativo = false;
    },
  }
}
</script>

<style>
.degrade {
  background: rgb(2, 0, 36);
  background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
  background-repeat: no-repeat;
  height: 100dvh;
}
</style>