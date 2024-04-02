<template>
  <Header />
  
  <div class="container">
    <Balance :total="total" /> 
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" 
    @transaction-deleted="handleTransactionDeleted"/> 
    <AddTransaction @transaction-submited="handleTransactionSubmited"/>

  </div>
</template>

<script setup>
  
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  
  import { useToast } from 'vue-toastification';

  import { ref , computed, onMounted } from 'vue';

  const toast = useToast();

  const transactions = ref([]);

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
      transactions.value = savedTransactions;
    }
  })

  // get total
  const total = computed(()=> {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.ammount;
    }, 0);
  });
  
  // get income

    const income = computed(()=> {
    return transactions.value
    .filter((transaction)=> transaction.ammount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.ammount;
    }, 0).toFixed(2);
  })

  // get expenses
  const expenses = computed(()=> {
    return transactions.value
    .filter((transaction)=> transaction.ammount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.ammount;
    }, 0).toFixed(2);
  })

  // add transaction
  const handleTransactionSubmited = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      ammount: transactionData.ammount

    });

    toast.success('Transaction added')
  }

  // generate unique ID
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
  }

  // delete transaction
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) =>
    transaction.id !== id);

    toast.success('Transaction deleted')
  }

</script>