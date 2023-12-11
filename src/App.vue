<template>
  <div>
    <h1>{{ orderName }}</h1>
    <input v-model="orderName" placeholder="Enter order name" />
    <button @click="submitOrder">Submit Order</button>
    <select v-model="currency">
      <option value="USD">Dollar $$</option>
      <option value="EUR">Euro €€</option>
    </select>
    <div v-for="product in convertedProducts" :key="product.name">
      <span>{{ product.name }} - {{ product.price }}{{ currency }}</span>
      <button @click="addToCart(product)">Add to Cart</button>
    </div>
    <br>
    <cart />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, provide } from 'vue';
import Cart from './components/Cart.vue';

const orderName = ref('');
const cartItems = ref([]);
const currency = ref('USD');
const exchangeRate = 0.95;

const products = ref([
  { name: "Hamburger 🍔", price: 5 },
  { name: "Cheeseburger 🧀", price: 6 },
  { name: "Impossible Burger 🥕", price: 7 },
  { name: "Fries 🍟", price: 2 },
]);

const convertedProducts = computed(() => {
  return products.value.map(product => ({
    ...product,
    price: currency.value === 'EUR' ? (product.price * exchangeRate).toFixed(2) : product.price
  }));
});

const addToCart = (product) => {
  const newItem = {
    ...product,
    price: currency.value === 'EUR' ? (product.price * exchangeRate).toFixed(2) : product.price
  };
  cartItems.value.push(newItem);

  const cartItemsDetails = cartItems.value.map(item => `${item.name} - ${currency.value}${item.price}`).join('\n');
  alert(`Added ${product.name} to cart!\n\nCurrent Cart:\n${cartItemsDetails}`);
};

const submitOrder = () => {
  if (!orderName.value.trim()){
    alert('Please enter a name for the order')
    return;
  }
  if (cartItems.value.length === 0) {
    alert('The order is empty.');
    return;
  }
  const orderDetails = cartItems.value.map(item => `${item.name} - ${currency.value}${item.price}`).join('\n');
  alert(`Order: ${orderName.value}\n\n${orderDetails}`);
  cartItems.value = [];
  orderName.value = ''
};

provide('cartItems', cartItems);
provide('currency', currency);
</script>
