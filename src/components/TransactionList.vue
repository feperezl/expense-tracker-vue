<template>
    <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="transaction in transactions"
        v-bind:key="transaction.id" 
        :class="transaction.ammount < 0 ? 'minus' : 'plus'">
        {{ transaction.text }} <span>$ {{ transaction.ammount }}</span><button class="delete-btn">x</button>
      </li>
      </ul>
  </template>
  
<script setup>

import { defineProps } from 'vue';

const props = defineProps({
    total: {
        type: Number,
        required: true,
    },
});
</script>