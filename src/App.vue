<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transaction-deleted="handleTransactionDeleted"/>
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
  import Header from "./components/Header.vue";
  import Balance from "./components/Balance.vue";
  import IncomeExpenses from "@/components/IncomeExpenses.vue";
  import TransactionList from "@/components/TransactionList.vue";
  import AddTransaction from "@/components/AddTransaction.vue";

  import { useToast } from "vue-toastification";

  import {ref, computed, onMounted} from 'vue';

  const toast = useToast();

  // get transactions
  const transactions = ref([]);
  /*const transactions = ref([
    { id: 1, text: 'Flower', amount: 200 },
    { id: 2, text: 'Ball', amount: 50 },
    { id: 3, text: 'Dog Food', amount: -100 },
  ]);*/

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
      transactions.value = savedTransactions;
    }
  });

  // get total
  const total = computed(() => {
    return transactions.value
        .reduce((accumulator, transaction) => {
          return accumulator + transaction.amount;
        }, 0);
  });

  // get income
  const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((accumulator, transaction) => {
          return accumulator + transaction.amount;
        }, 0)
        .toFixed(2);
  });

  // get expenses
  const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((accumulator, transaction) => {
          return accumulator + transaction.amount;
        }, 0)
        .toFixed(2);
  });

  // add transaction
  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    });

    saveTransactionsToLocalStorage();

    toast.success('Transaction added');
  };

  // delete transaction
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

    saveTransactionsToLocalStorage();
    toast.success('Transaction deleted');
  };

  // save to localstorage
  const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions));
  };

  // generate unique id
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 100000);
  };

  /*export default defineComponent({
    components: {
      Header,
      Balance,
      IncomeExpenses,
      TransactionList,
      AddTransaction
    },
  });*/

</script>