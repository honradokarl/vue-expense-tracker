<script setup>

  import { defineProps } from 'vue';

  const props = defineProps({
    transactions: {
      type: Array,
      required: true
    },
  });

  // Composition API
  // const transactions = [
  //   { id: 1, text: 'Flower', amount: 100 },
  //   { id: 2, text: 'Ball', amount: 50 },
  //   { id: 3, text: 'Dog Food', amount: 100 },
  // ]

  // Composition API
  // export default {
  //   setup() {
  //     const transactions = [
  //       { id: 1, text: 'Flower', amount: 100 },
  //       { id: 2, text: 'Ball', amount: 50 },
  //       { id: 3, text: 'Dog Food', amount: 100 },
  //     ];
  //
  //     return {
  //       transactions
  //     };
  //   }
  // }

  // This is options API
  // export default {
  //   data() {
  //     return {
  //       transactions: [
  //         { id: 1, text: 'Flower', amount: -19.99 },
  //         { id: 1, text: 'Ball', amount: -19.99 },
  //         { id: 1, text: 'Dog Food', amount: -19.99 },
  //       ],
  //     }
  //   },
  // }
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-for="transaction in transactions"
        :key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'">
      {{ transaction.text }} <span>{{ transaction.amount }}</span><button class="delete-btn">x</button>
    </li>
  </ul>
</template>

<style scoped>

</style>