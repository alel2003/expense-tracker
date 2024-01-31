<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="number" id="amount" v-model="number" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>
<script setup>
import { ref } from 'vue';
import {useToast} from 'vue-toastification'
import { defineEmits } from 'vue';

const text = ref('')
const number = ref('')

const toast = useToast()

const emit = defineEmits(['add-list'])

const onSubmit = () => {
    if(!text.value || !number.value) {
        toast.error('Both fields must be filled')
        return 
    }
    const transactionData = {
        id: Date.now(),
        text: text.value,
        amount: parseFloat(number.value)
    }
    emit('add-list', transactionData)
    text.value = ''
    number.value = ''
    console.log(transactionData)
}
</script>
<style scoped></style>