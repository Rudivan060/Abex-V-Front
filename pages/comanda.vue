<template>
  <div class="degrade">
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

    <div class="comanda-outer">
      <v-card class="comanda-card">
        <div class="container">
          <h1>Comanda</h1>
          <form id="order-form">
            <div
              v-for="(item) in outFood"
              :key="item.name"
              class="coffee-item"
            >
              <label>
                {{ item.nome }} - ${{ item.valor }}
              </label>
              <div style="text-align: right;">
                <input v-model.number="item.quantidade" style="width: 5%; text-align: center;" min="0" :disabled="!item.checked">
              </div>
            </div>
          </form>

          <div id="order-list">
            <p>Total vendido: <span class="total">R${{ totalAmounts }}</span></p>
          </div><br>
          
          <h2>Qual mesa meu guri?</h2>

          <div class="form-actions">
            <v-row>
              <v-col>
                <v-autocomplete 
                  v-model="token.id"
                  :items="token" 
                  placeholder="Mesa"
                  item-title="id" 
                  item-value="id"
                />
              </v-col>
            </v-row>
          </div>
        </div>
      </v-card>
    </div>
  </div>
</template>
    
<script>
  export default {
    data() {
      return {
        tab: 3,
        outFood: [],
        produto: [],
        token: [],
        orders: [],
        totalAmount: 0,
        productTotals: {},
      };
    },

    computed: {
      totalAmounts() {
        return this.outFood.reduce((total, item) => {
          return total + (item.valor * item.quantidade);
        }, 0);
      }
    },

    watch: {
      'token.id'(novoId) {
        if (novoId) {
          this.getComanda();
        }
      }
    },

    created() {
      this.getProduto();
      this.getComanda();
      this.getToken();
    },

    methods: {
      increase(idx) {
        if (this.outFood[idx].checked) this.outFood[idx].quantity++;
      },
      
      decrease(idx) {
        if (this.outFood[idx].checked && this.outFood[idx].quantity > 0)
          this.outFood[idx].quantity--;
      },

      async getComanda() {
        this.loading = true;
        try {
          const response = await this.$api.get(`/pedido/produto/${this.token.id}`);
          this.outFood = response.response.map((item) => {
            const produto = this.produto.find((p) => p.id === item.idProduto);
            return {
              ...item,
              nome: produto ? produto.nome : "Desconhecido",
              valor: produto ? produto.valor : 0,
            };
          });
        } catch (error) {
          console.error("Erro ao carregar itens:", error);
        } finally {
          this.loading = false;
          console.log("dados carregados");
        }
      },

      async getProduto() {
        this.loading = true;
        try {
          const response = await this.$api.get("/produto");
          this.produto = response.response;
        } catch (error) {
          console.error("Erro ao carregar itens:", error);
        } finally {
          this.loading = false;
          console.log("dados carregados");
        }
      },

      async getToken() {
        this.loading = true;
        try {
          const response = await this.$api.get("/token");
          this.token = response.response;
        } catch (error) {
          console.error("Erro ao carregar token:", error);
        } finally {
          this.loading = false;
          console.log('gayy', this.token);
          console.log("token carregado");
        }
      },
    }
  };
</script>

<style>
  body {
    font-family: Arial, sans-serif;
    background-color: #f7f3e9;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  .comanda-outer {
    min-height: 100vh;
    width: 100vw;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding-top: 40px;
    box-sizing: border-box;
  }

  .comanda-card {
    width: 40%;
    min-height: 500px;
    padding: 32px 24px;
    border-radius: 16px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  .container {
    width: 70%;
    max-width: 900px;
    margin: 0 auto;
    padding: 20px 0;
    background: transparent;
    box-shadow: none;
  }

  h1,
  h2 {
    color: #6b4f4f;
    text-align: center;
  }

  .coffee-item {
    display: flex;
    justify-content: space-between; 
    margin-bottom: 15px;
    padding: 10px;
    border-bottom: 1px solid #ddd;
  }
  
  .quantity-btn:hover {
    background-color: #b2846b;
  }

  input[type="number"] {
    width: 50px;
    text-align: center;
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 5px;
  }

  .form-actions {
    text-align: center;
    margin-top: 20px;
  }

  button#generate-order,
  button#export-csv {
    background-color: #6b4f4f;
    color: #fff;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button#generate-order:hover,
  button#export-csv:hover {
    background-color: #543c3c;
  }

  #order-list {
    margin-top: 30px;
  }

  #orders {
    list-style-type: none;
    padding: 0;
  }

  #orders li {
    background-color: #f3e1d6;
    margin-bottom: 10px;
    padding: 10px;
    border-radius: 4px;
  }

  .alert {
    background-color: #ffeeba;
    padding: 10px;
    border: 1px solid #ffc107;
    border-radius: 4px;
    margin-bottom: 10px;
  }

  .no-display {
    display: none;
  }

  .degrade {
    background: linear-gradient(0deg, rgba(2, 0, 36, 1) 0%, rgba(182, 52, 25, 1) 0%, rgba(255, 190, 0, 1) 100%);
    background-repeat: no-repeat;
    background-size: cover;
    min-height: 100vh;
    z-index: 0;
    overflow-y: auto;
  }

  .total {
    font-weight: bold;
    font-size: 1.5rem;
  }
</style>