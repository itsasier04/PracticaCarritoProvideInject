<template>
    <div v-if="items.length">
      <h2>Cart</h2>
      <ul>
        <li v-for="item in items" :key="item.name">
            <p>{{ item.name }} - {{ item.price }}{{ currency }}</p>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup lang="ts">
  import { inject } from 'vue';
  
  const items = inject('cartItems');
  const currency = inject('currency');
  </script>
  