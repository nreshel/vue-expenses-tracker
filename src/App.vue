<template>
  <div class="container">
    <h1>Expense Tracker</h1>
    <div class="balance">
      <h2>Balance: {{ balance }}$</h2>
    </div>
    <div class="form">
      <input type="text" placeholder="Description" v-model="description" />
      <input type="number" placeholder="Amount" v-model="amount" />
      <button @click="addExpense">Add Expense</button>
      <button @click="addIncome">Add Income</button>
    </div>
    <div class="transactions">
      <h3>Transactions</h3>
      <ul>
        <li v-for="(transaction, index) in transactions" :key="index">
          {{ transaction.description }}: {{ transaction.amount }}$
          <button @click="deleteTransaction(index)">X</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// Reactive references for description, amount, and transactions
const description = ref("");
const amount = ref(0);
const transactions = ref([]);

// Computed property for balance
const balance = computed(() => {
  // Sum up the amounts of all transactions
  return transactions.value.reduce(
    (total, transaction) => total + transaction.amount,
    0
  );
});

// Function to add an expense transaction
const addExpense = () => {
  // Validate the input fields
  if (description.value && amount.value) {
    // Create a new transaction object with a negative amount
    const transaction = {
      description: description.value,
      amount: -Math.abs(amount.value),
    };
    // Add the transaction to the transactions array
    transactions.value.push(transaction);
    // Clear the input fields
    description.value = "";
    amount.value = 0;
  }
};

// Function to add an income transaction
const addIncome = () => {
  // Validate the input fields
  if (description.value && amount.value) {
    // Create a new transaction object with a positive amount
    const transaction = {
      description: description.value,
      amount: Math.abs(amount.value),
    };
    // Add the transaction to the transactions array
    transactions.value.push(transaction);
    // Clear the input fields
    description.value = "";
    amount.value = 0;
  }
};

// Function to delete a transaction by index
const deleteTransaction = (index) => {
  // Remove the transaction from the transactions array
  transactions.value.splice(index, 1);
};
</script>

<style>
/* Add some styles here */
.container {
  max-width: 600px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
}

.balance {
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 10px;
  margin: 20px 0;
}

.balance h2 {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}

.form {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 20px 0;
}

.form input {
  width: 40%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
}

.form button {
  width: 10%;
  padding: 10px;
  border: none;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  padding: 15px;
  width: 15%;
  margin: 5px;
}

.form button:hover {
  opacity: 0.8;
}

.form button:nth-child(3) {
  background-color: #f44336;
}

.form button:nth-child(4) {
  background-color: #4caf50;
}

.transactions {
  background-color: #f0f0f0;
  padding: 20px;
  border-radius: 10px;
  margin: 20px 0;
}

.transactions h3 {
  text-align: center;
  font-size: 18px;
  font-weight: bold;
}

.transactions ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.transactions li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

.transactions li:last-child {
  border-bottom: none;
}

.transactions li button {
  width: 20px;
  height: 20px;
  border: none;
  border-radius: 50%;
  background-color: #f44336;
  color: #fff;
  font-size: 12px;
  font-weight: bold;
  cursor: pointer;
  padding: 5px;
}

.transactions li button:hover {
  opacity: 0.8;
}

</style>
