<template>
  <div class="container">
    <Header />
    <Balance :total="total"/>
    <IncomeExpenses :income="income" :expenses="parseFloat(expenses)"/>
    <TransactionList :transactions="transactions" @delete-list="deleteNote"/>
    <AddTransactionVue @add-list="newList"/>
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransactionVue from './components/AddTransaction.vue';

import { ref, computed, onMounted } from 'vue';

import  { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([]);
onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem(('transactions')))
  if(saveTransactions) {
    transactions.value = saveTransactions
  }
})

// get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount
    }, 0)
})

// get income
const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount
    }, 0).toFixed(2)
})

// get expenses
const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount
    }, 0).toFixed(2)
})

// add list 
const newList = (list) => {
  transactions.value.push({
    id: list.id,
    text: list.text,
    amount:list.amount
  })
  saveTransactionsLocalStorage()
  toast.success('Transition added')
}

// delete list 
const deleteNote = (id) => {
  transactions.value = transactions.value.filter(transaction => transaction.id !== id)
  saveTransactionsLocalStorage()
  toast.success('Transaction delete')
}

// save localStorage
const saveTransactionsLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
} 
</script>

