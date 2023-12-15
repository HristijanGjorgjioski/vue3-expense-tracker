<template>
  <Header />

  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      @transactionDeleted="handleTransactionDeleted"
      :transactions="transactions" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();
const transactions = ref([{ id: 3, text: 'Groceries', amount: 41.28 }]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({ id: generateUniqueId(), ...transactionData });
  toast.success('Transaction added!');
};

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  toast.success('Transaction deleted!');
};
</script>

