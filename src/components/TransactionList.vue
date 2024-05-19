<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <!-- <li class="minus">
            Cash <span>$400</span> <button class="delete-btn">X</button>
        </li>
        <li class="plus">
            Paycheck <span>$400</span> <button class="delete-btn">X</button>
        </li> -->

        <li  v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0? 'minus': 'plus'">
           {{transaction.text}} <span>${{transaction.amount}}</span> <button class="delete-btn" @click="deleteTransaction(transaction.id)">X</button>
        </li> 
    </ul>
</template>


<script setup>
    import { defineProps } from 'vue';

    const emit =defineEmits(['transactionDeleted']);


    const props= defineProps({
        transactions:{
            type:Array,
            required: true
        },
    });

    const deleteTransaction = (id) =>{
        emit('transactionDeleted', id);
    }
</script>

<!-- <script setup>
    const transactions = [
        { id:1, text:'Flower', amount: -19.99},
        { id:2, text:'Salary', amount: 299.99},
        { id:3, text:'Book', amount: -8.99},
        { id:4, text:'Monitor', amount: -15.99},
        { id:5, text:'Camera', amount: -150.99}
    ];
</script> -->



<!-- <script>
    export default{
       setup(){
        const transactions = [
                    { id:1, text:'Flower', amount: -19.99},
                    { id:2, text:'Salary', amount: 299.99},
                    { id:3, text:'Book', amount: -8.99},
                    { id:4, text:'Monitor', amount: -15.99},
                    { id:5, text:'Camera', amount: -150.99}
                ];
                return {
                    transaction,
                }
       }
    }
</script> -->