<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount >= 0 ? 'plus' : 'minus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>
<script setup>
import { computed, defineProps } from "vue";

const emit = defineEmits(["transactionDeletedId"]);

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

function deleteTransaction(id) {
  emit("transactionDeletedId", id);
  //props.transactions.value = props.transactions.value.find((transaction) => transaction.value == id);
}
</script>
