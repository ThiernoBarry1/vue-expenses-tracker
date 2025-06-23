<template>
  <Header></Header>
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="income" :expense="expense" />
    <TransactionList
      :transactions="transactions"
      @transactionDeletedId="handleDeleteTransaction"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, onMounted, ref } from "vue";
import { useToast } from "vue-toastification";

const transactions = ref([]);
onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (saveTransactions) {
    transactions.value = saveTransactions;
  }
});
// get Total
const total = computed(() => {
  return transactions.value
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
// get Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
//get Expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
const toast = useToast();

// Add transaction to transactions
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: genereateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  saveTransactionsToLocalStorage();

  toast.success("Transaction added !");

  console.log(transactionData);
};
// Generate unique ID
const genereateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};
// Delete transaction
const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  toast.success("transaction deleted!");
};

// Save to localstorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
