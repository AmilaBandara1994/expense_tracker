
<template>
  <Header/>
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense  :income="+income" :expense="+expense"/>
    <TransactionList :transactions="transactions"  @transactionDeleted="handleTransactionaDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionaSubmitted" />
  </div>
</template>

 <!-- new way of  -->
<script setup>
    import AddTransaction from './components/AddTransaction.vue';
    import Balance from './components/Balance.vue';
    import Header from './components/Header.vue';
    import IncomeExpense from './components/IncomeExpense.vue';
    import TransactionList from './components/TransactionList.vue';

    import { ref, computed, onMounted } from 'vue';
    import { useToast } from 'vue-toastification';

    const toast = useToast();

    const transactions = ref([]);

    onMounted(()=>{
        const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

        if(savedTransactions){
            transactions.value = savedTransactions;
        }
    })

    // get total 
    const total = computed(() =>{
        return transactions.value.reduce((acc, transaction)=>{
            return acc + transaction.amount;
        },0).toFixed(2);
    });

    // get income 
    const income = computed(() =>{
        return transactions.value
        .filter((tran) => tran.amount > 0)
        .reduce((acc, transaction)=>{
            return acc + transaction.amount;
        },0).toFixed(2);
    });
    // get expense 
    const expense = computed(() =>{
        return transactions.value
        .filter((tran)=> tran.amount < 0 )
        .reduce((acc, transaction)=>{
            return acc + transaction.amount;
        },0).toFixed(2);
    });



    //add transaction 

    const handleTransactionaSubmitted = (transactionData) => {
        transactions.value.push({
            id: generateUniqId(),
            text: transactionData.text,
            amount: transactionData.amount
        })
        saveTransactionsToLocalStorage();
       toast.success("Successfully Added Transaction.");
    }

    // delete transaction

    const handleTransactionaDeleted = (id) => {
        transactions.value = transactions.value.filter( (trans) => trans.id != id);

        saveTransactionsToLocalStorage();
        toast.success("Transaction Deleted");
    }


    //generrate id 

    const  generateUniqId = () =>{
        return Math.floor(Math.random() * 100000);
    }


    //save to localstorage
    const saveTransactionsToLocalStorage =() =>{
        localStorage.setItem('transactions', JSON.stringify(transactions.value));
    }

</script>





<!-- { id:1, text:'Flower', amount: -19.99},
{ id:2, text:'Salary', amount: 299.99},
{ id:3, text:'Book', amount: -8.99},
{ id:4, text:'Monitor', amount: -15.99},
{ id:5, text:'Camera', amount: -150.99} -->







<!-- <script>
    old away of define script
import AddTransaction from './components/AddTransaction.vue';
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';

    

export default{
    components: {
        Header,
        Balance,
        IncomeExpense,
        TransactionList,
        AddTransaction
    }
} 
</script> -->