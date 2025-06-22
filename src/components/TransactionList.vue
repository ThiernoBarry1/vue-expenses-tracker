<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount >= 0 ? 'plus' : 'minus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteItem(transaction.id)">x</button>
    </li>
  </ul>
</template>
<script setup>
const transactions = [
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.97 },
  { id: 3, text: "Book", amount: -10.99 },
  { id: 4, text: "Camera", amount: 150.99 },
];
</script>
