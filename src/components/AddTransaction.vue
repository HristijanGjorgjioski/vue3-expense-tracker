<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        v-model="amount"
        type="text"
        id="amount"
        placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const emit = defineEmits(['transactionSubmitted']);
const toast = useToast();

const text = ref();
const amount = ref();

const clearForm = () => {
  text.value = '';
  amount.value = '';
};

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Both fields are required');
    return;
  }

  const transactionData = {
    amount: parseFloat(amount.value),
    text: text.value,
  };

  emit('transactionSubmitted', transactionData);

  clearForm();
};
</script>

