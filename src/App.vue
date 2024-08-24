<script setup>
import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

import TheHeader from './components/TheHeader.vue'
import TheBalance from './components/TheBalance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

const transactions = ref([
  { id: 1, text: 'Flower', amount: -19.99 },
  { id: 2, text: 'Salary', amount: 299.97 },
  { id: 3, text: 'Book', amount: -10 },
  { id: 4, text: 'Camera', amount: 150 }
])

const toast = useToast()

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

// Generate Unique ID
function generateUniqueId() {
  return Math.floor(Math.random() * 1000000)
}

// Add Transaction
function handleAddTrasaction(transaction) {
  transactions.value.push({
    id: generateUniqueId(),
    text: transaction.text,
    amount: transaction.amount
  })
  saveTransactionToLocalStorage()
  toast.success('Transaction added')
}

// Delete Transaction
function handleDeleteTransaction(id) {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  )
  saveTransactionToLocalStorage()
  toast.success('Transaction deleted')
}

// Save to localstorage
function saveTransactionToLocalStorage() {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
  <TheHeader />
  <div class="container">
    <TheBalance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @delete-transaction="handleDeleteTransaction"
    />
    <AddTransaction @add-transaction="handleAddTrasaction" />
  </div>
</template>

<style scoped></style>
