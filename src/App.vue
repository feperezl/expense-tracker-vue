<template>
  <Header />
  
  <div class="container">
    <Balance :total="total" /> 
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" /> 
    <AddTransaction />

  </div>
</template>

<script setup>
  
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  
  import { ref , computed} from 'vue';

  const transactions = ref([
    { id: 1, text: 'Flowers', ammount: -19.99 },
    { id: 2, text: 'TV', ammount: -229.99 },
    { id: 3, text: 'Salary', ammount: 322.99 }
  ]);

  // get total
  const total = computed(()=> {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.ammount;
    }, 0)
  })
  
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


</script>