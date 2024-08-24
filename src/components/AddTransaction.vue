<script setup>
import { defineEmits, ref } from 'vue'
import { useToast } from 'vue-toastification'

const text = ref('')
const amount = ref('')

const toast = useToast()

const emit = defineEmits(['add-transaction'])

function onSubmit() {
  if (!text.value || !amount.value) {
    toast.error('Both fields must be filled')
    return
  }

  const transactionData = {
    text: text.value,
    amount: amount.value
  }

  emit('add-transaction', transactionData)

  text.value = ''
  amount.value = ''
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        id="text"
        placeholder="Enter text..."
        v-model.trim="text"
      />
    </div>
    <div class="form-control">
      <label for="amount">
        Amount <br />
        (negative - expense, positive - income)
      </label>
      <input
        type="number"
        id="amount"
        placeholder="Enter amount..."
        v-model.number="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
