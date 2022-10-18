<template>
  <div style="text-align: center">
    <h1>My Ledger</h1>
    <div class="mb-4">
      <input
        type="number"
        v-model="amountPaid"
        placeholder="Enter amount in Dollar"
      />
    </div>
    <div class="mb-4">
      <select v-model="selectedCategory">
        <option
          v-for="option in ledgerOptions"
          :key="option.value"
          :value="option.value"
        >
          {{ option.label }}
        </option>
      </select>
    </div>
    <div class="mb-4">Balance : {{ balance }}</div>

    <button @click="amountPaidSubmit">Amount Paid</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      balance: 0,
      allPaid: 0,
      amountPaid: 0,
      selectedCategory: "tuition",
      ledgerOptions: [
        {
          label: "Tuition",
          value: "tuition",
        },
        {
          label: "Electricity",
          value: "electricity",
        },
        {
          label: "Rent",
          value: "rent",
        },
      ],

      ledgerBook: {
        tuition: {
          totalDue: 3000,
          paid: [],
        },
        electricity: {
          totalDue: 5000,
          paid: [],
        },
        rent: {
          totalDue: 8000,
          paid: [1000,],
        },
      },
    };
  },
  mounted() {
    // this.calculateBalance();
  },
  watch: {
    ledgerBook: {
      handler() {
        this.calculateBalance();
      },
      deep: true,
      immediate:true,
    },
  },
  methods: {
    amountPaidSubmit() {
      this.ledgerBook[this.selectedCategory].paid.push(this.amountPaid);
    },
    calculateBalance() {
      for (const property in this.ledgerBook) {
        this.ledgerBook[property].paid.forEach((item) => {
          this.allPaid += item;
        });
      }
      const totalDue =
        this.ledgerBook.tuition.totalDue +
        this.ledgerBook.electricity.totalDue +
        this.ledgerBook.rent.totalDue;
      this.balance = totalDue - this.allPaid;
    },
  },
};
</script>

<style>
.mb-4 {
  margin-bottom: 12px;
}
</style>
