<template>
  <div id="app">
    <img src="./assets/logo.png" alt class="money-img" />
    <h1>Währungsrechner</h1>
    <div class="container">
      <div class="currency">
        <select @change="change" v-model="selected_1">
          <option value="AED">AED</option>
          <option value="ARS">ARS</option>
          <option value="AUD">AUD</option>
          <option value="BGN">BGN</option>
          <option value="BRL">BRL</option>
          <option value="BSD">BSD</option>
          <option value="CAD">CAD</option>
          <option value="CHF">CHF</option>
          <option value="CLP">CLP</option>
          <option value="CNY">CNY</option>
          <option value="COP">COP</option>
          <option value="CZK">CZK</option>
          <option value="DKK">DKK</option>
          <option value="DOP">DOP</option>
          <option value="EGP">EGP</option>
          <option value="EUR">EUR</option>
          <option value="FJD">FJD</option>
          <option value="GBP">GBP</option>
          <option value="GTQ">GTQ</option>
          <option value="HKD">HKD</option>
          <option value="HRK">HRK</option>
          <option value="HUF">HUF</option>
          <option value="IDR">IDR</option>
          <option value="ILS">ILS</option>
          <option value="INR">INR</option>
          <option value="ISK">ISK</option>
          <option value="JPY">JPY</option>
          <option value="KRW">KRW</option>
          <option value="KZT">KZT</option>
          <option value="MXN">MXN</option>
          <option value="MYR">MYR</option>
          <option value="NOK">NOK</option>
          <option value="NZD">NZD</option>
          <option value="PAB">PAB</option>
          <option value="PEN">PEN</option>
          <option value="PHP">PHP</option>
          <option value="PKR">PKR</option>
          <option value="PLN">PLN</option>
          <option value="PYG">PYG</option>
          <option value="RON">RON</option>
          <option value="RUB">RUB</option>
          <option value="SAR">SAR</option>
          <option value="SEK">SEK</option>
          <option value="SGD">SGD</option>
          <option value="THB">THB</option>
          <option value="TRY">TRY</option>
          <option value="TWD">TWD</option>
          <option value="UAH">UAH</option>
          <option value="USD">USD</option>
          <option value="UYU">UYU</option>
          <option value="VND">VND</option>
          <option value="ZAR">ZAR</option>
        </select>
        <input
          v-model.number="value_1"
          type="number"
          placeholder="1"
          @input="calculate"
          id="1"
        />
      </div>
      <div class="swap-rate-container">
        <button class="btn" id="swap" v-on:click="swap">Tauschen</button>
        <div class="rate" id="rate">
          1 {{ selected_1 }} = {{ rate }} {{ selected_2 }}
        </div>
      </div>
      <div class="currency">
        <select v-model="selected_2" @change="change">
          <option value="AED">AED</option>
          <option value="ARS">ARS</option>
          <option value="AUD">AUD</option>
          <option value="BGN">BGN</option>
          <option value="BRL">BRL</option>
          <option value="BSD">BSD</option>
          <option value="CAD">CAD</option>
          <option value="CHF">CHF</option>
          <option value="CLP">CLP</option>
          <option value="CNY">CNY</option>
          <option value="COP">COP</option>
          <option value="CZK">CZK</option>
          <option value="DKK">DKK</option>
          <option value="DOP">DOP</option>
          <option value="EGP">EGP</option>
          <option value="EUR">EUR</option>
          <option value="FJD">FJD</option>
          <option value="GBP">GBP</option>
          <option value="GTQ">GTQ</option>
          <option value="HKD">HKD</option>
          <option value="HRK">HRK</option>
          <option value="HUF">HUF</option>
          <option value="IDR">IDR</option>
          <option value="ILS">ILS</option>
          <option value="INR">INR</option>
          <option value="ISK">ISK</option>
          <option value="JPY">JPY</option>
          <option value="KRW">KRW</option>
          <option value="KZT">KZT</option>
          <option value="MXN">MXN</option>
          <option value="MYR">MYR</option>
          <option value="NOK">NOK</option>
          <option value="NZD">NZD</option>
          <option value="PAB">PAB</option>
          <option value="PEN">PEN</option>
          <option value="PHP">PHP</option>
          <option value="PKR">PKR</option>
          <option value="PLN">PLN</option>
          <option value="PYG">PYG</option>
          <option value="RON">RON</option>
          <option value="RUB">RUB</option>
          <option value="SAR">SAR</option>
          <option value="SEK">SEK</option>
          <option value="SGD">SGD</option>
          <option value="THB">THB</option>
          <option value="TRY">TRY</option>
          <option value="TWD">TWD</option>
          <option value="UAH">UAH</option>
          <option value="USD">USD</option>
          <option value="UYU">UYU</option>
          <option value="VND">VND</option>
          <option value="ZAR">ZAR</option>
        </select>
        <input
          v-model.number="value_2"
          type="number"
          @input="calculate"
          id="2"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  components: {},
  data() {
    return {
      rate: null,
      selected_1: 'USD',
      selected_2: 'EUR',
      value_1: 1,
      value_2: null
    };
  },
  created() {
    axios
      .get(`https://api.exchangerate-api.com/v4/latest/${this.selected_1}`)
      .then(res => {
        this.rate = res.data.rates[this.selected_2];
        this.value_2 = (this.rate * this.value_1).toFixed(2);
      });
  },
  methods: {
    change() {
      axios
        .get(`https://api.exchangerate-api.com/v4/latest/${this.selected_1}`)
        .then(res => {
          console.log(res);
          this.rate = res.data.rates[this.selected_2];
          this.value_2 = (this.rate * this.value_1).toFixed(2);
        });
    },
    calculate(event) {
      if (event.target.id === '1') {
        this.value_2 = (this.rate * event.target.value).toFixed(2);
      } else {
        this.value_1 = (this.rate * event.target.value).toFixed(2);
      }
    },
    swap() {
      const temp = this.selected_1;
      this.selected_1 = this.selected_2;
      this.selected_2 = temp;
      this.rate = (1 / this.rate).toFixed(6);
      this.value_2 = (this.rate * this.value_1).toFixed(2);
    }
  }
};
</script>

<style>
@import './assets/variables.css';
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #616161;
  font-family: Arial, Helvetica, sans-serif;
  display: flex; /* allings all horizontally thats why: */
  flex-direction: column;
  align-items: center;
  justify-content: center; /* align in the center vertically*/
  height: 100vh;
  margin: 0;
  padding: 20px;
}
* {
  box-sizing: border-box;
}

body {
  background-color: #616161;
  font-family: Arial, Helvetica, sans-serif;
  display: flex; /* allings all horizontally thats why: */
  flex-direction: column;
  align-items: center;
  justify-content: center; /* align in the center vertically*/
  height: 100vh;
  margin: 0;
  padding: 20px;
}

h1 {
  color: var(--primary-color);
}

.btn {
  color: #fff;
  background: var(--primary-color);
  cursor: pointer;
  border: none;
  border-radius: 5px;
  font-size: 12px;
  padding: 5px 12px; /* packt einen bereich zwischen text und btn rand um den Text besser in die mitte zu schreiben */
}

.money-img {
  width: 150px;
}

.currency {
  padding: 30px 0; /* abstand zwischen den elementen */
  display: flex; /* align everything correctly */
  align-items: center;
  justify-content: space-between; /* takes remaining space and puts it between items */
}

.currency select {
  padding: 10px 20px 10px 10px;
  -moz-appearance: none; /*mac and windows have some default values for buttons. we can disable them like this */
  -webkit-appearance: none;
  appearance: none;
  color: #fff;
  border: 1px solid #dedede;
  font-size: 16px;
  background: transparent;
  background-color: #616161;
  /* background svg for buttons */
  background-image: url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%20000002%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E');
  background-position: right 10px top 50%, 0, 0;
  background-size: 12px auto, 100%;
  background-repeat: no-repeat;
  border-radius: 5px;
}

input[type='number']::-webkit-inner-spin-button,
input[type='number']::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
  outline: none;
}
input[type='number'] {
  -moz-appearance: textfield;
  outline: none;
  border: none;
}
.currency input {
  background: transparent;
  border: none;
  font-size: 30px;
  text-align: right;
  color: var(--primary-color);
}

.swap-rate-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.rate {
  color: var(--primary-color);
  font-size: 14px;
  padding: 0 10px;
}

/* removes blue outline around the buttons */
select:focus,
input:focus,
textfield:focus,
button:focus {
  outline: none;
  border: none;
}

.btn:hover {
  transform: scale(1.1);
}
</style>
