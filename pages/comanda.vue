<template>
  <div class="container">
    <h1>Dolce Fabrizio</h1>
    <h2>Generador de Comandas</h2>
    <div class="alert">
      <p>NO CIERRES AL VENTANA QUE SE PIERDEN LOS DATOS.</p>
    </div>
    <form id="order-form">
      <div
        class="coffee-item"
        v-for="(item, idx) in coffeeItems"
        :key="item.name"
      >
        <label>
          <input type="checkbox" v-model="item.checked">
          {{ item.name }} (${{ item.price }})
        </label>
        <div class="quantity-controls">
          <button type="button" class="quantity-btn" @click="decrease(idx)">-</button>
          <input type="number" min="0" v-model.number="item.quantity" :disabled="!item.checked">
          <button type="button" class="quantity-btn" @click="increase(idx)">+</button>
        </div>
      </div>
      <div class="form-actions">
        <button type="button" @click="generateOrder">Generar Comanda</button>
      </div>
    </form>
    <div id="order-list">
      <h2>Listado de Comandas</h2>
      <ul>
        <li v-for="order in orders" :key="order.date">
          <p><strong>{{ order.date }}</strong></p>
          <div v-for="item in order.items" :key="item.name">
            <p>{{ item.quantity }} x {{ item.name }} (${{ item.price }})</p>
          </div>
          <p><strong>Total: ${{ order.total }}</strong></p>
        </li>
      </ul>
      <p>Total vendido hoy: $<span class="total">{{ totalAmount }}</span></p>
      <p>
        <span v-for="(total, name) in productTotals" :key="name">
          {{ name }}: ${{ total }}<br />
        </span>
      </p>
    </div>
    <div class="form-actions">
      <button type="button" @click="exportCSV">Exportar CSV</button>
    </div>
  </div>
</template>
    
<script>
  export default {
    data() {
      return {
        coffeeItems: [
          { name: "Espresso.", price: 10, checked: false, quantity: 0 },
          { name: "Espresso doble", price: 20, checked: false, quantity: 0 },
          { name: "Latte", price: 30, checked: false, quantity: 0 },
          { name: "Capuchino", price: 35, checked: false, quantity: 0 },
          { name: "Flat white", price: 40, checked: false, quantity: 0 },
          { name: "Mocha", price: 40, checked: false, quantity: 0 },
          { name: "Chocolate", price: 40, checked: false, quantity: 0 },
          { name: "Chai", price: 40, checked: false, quantity: 0 },
          { name: "Red velvet", price: 40, checked: false, quantity: 0 }
        ],
        orders: [],
        totalAmount: 0,
        productTotals: {},
      };
    },
    methods: {
      increase(idx) {
        if (this.coffeeItems[idx].checked) this.coffeeItems[idx].quantity++;
      },
      decrease(idx) {
        if (this.coffeeItems[idx].checked && this.coffeeItems[idx].quantity > 0)
          this.coffeeItems[idx].quantity--;
      },
      generateOrder() {
        const order = {
          date: new Date().toLocaleString(),
          items: [],
          total: 0
        };
        this.coffeeItems.forEach((item) => {
          if (item.checked && item.quantity > 0) {
            order.items.push({
              name: item.name,
              quantity: item.quantity,
              price: item.price * item.quantity
            });
            order.total += item.price * item.quantity;
            this.productTotals[item.name] =
              (this.productTotals[item.name] || 0) + item.price * item.quantity;
          }
        });
        if (order.items.length > 0) {
          this.orders.push(order);
          this.totalAmount += order.total;
          // Limpa seleção
          this.coffeeItems.forEach((item) => {
            item.checked = false;
            item.quantity = 0;
          });
        }
      },
      exportCSV() {
        let csvContent = "data:text/csv;charset=utf-8,";
        csvContent += "Date,Item,Quantity,Price,Total\n";
        this.orders.forEach((order) => {
          order.items.forEach((item) => {
            csvContent += `${order.date},${item.name},${item.quantity},${
              item.price / item.quantity
            },${item.price}\n`;
          });
          csvContent += `,,,Total,${order.total}\n`;
        });
        csvContent += `\nTotal Vendido,,,$,${this.totalAmount}\n`;
        for (const [product, total] of Object.entries(this.productTotals)) {
          const item = this.coffeeItems.find((i) => i.name === product);
          csvContent += `${product},,,${
            item ? total / item.price : 0
          },${total}\n`;
        }
        const encodedUri = encodeURI(csvContent);
        const link = document.createElement("a");
        link.setAttribute("href", encodedUri);
        link.setAttribute("download", "comandas.csv");
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      }
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

  .container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  h1,
  h2 {
    color: #6b4f4f;
    text-align: center;
  }

  .coffee-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
    padding: 10px;
    border-bottom: 1px solid #ddd;
  }

  .quantity-controls {
    display: flex;
    align-items: center;
  }

  .quantity-btn {
    background-color: #d7a48f;
    border: none;
    padding: 5px 10px;
    margin: 0 5px;
    cursor: pointer;
    color: #fff;
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

  .total {
    font-weight: bold;
    font-size: 1.5rem;
  }
</style>