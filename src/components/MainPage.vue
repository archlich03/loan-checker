<template>
  <form @submit.prevent="submitForm" class="form">
    <label>
      Monthly income (&euro;):
      <input type="number" v-model="form.income" min="0" max="10000" class="input-field" required />
    </label>
    <label>
      Age:
      <input type="number" v-model="form.age" min="1" max="100" class="input-field" required />
    </label>
    <label>
      Credit Score:
      <input type="number" v-model="form.creditScore" min="0.00" max="100.00" step="0.01" class="input-field" required/>
    </label>
    <button type="submit" class="submit-button">Submit</button>
    <p v-if="result" class="result">{{ result }}</p>
  </form>
</template>

<script>
export default {
  data() {
    return {
      form: {
        income: 0,
        age: 0,
        creditScore: 0
      },
      result: null
    }
  },
  methods: {
    submitForm() {
      let income = this.form.income;
      let age = this.form.age;
      let creditScore = this.form.creditScore;

      /*
        A - 0,00% - 0,44% really low risk
        B - 0,45% - 1,71% low risk
        C - 1,72% - 5,04% standard risk
        D - 5,05% - 21,9% high risk
        E - 21,91% - 100% really high risk
      */
      let acceptableRisk = 10; // how much risk can we tolerate
      let shouldLoan = "";

      // recheck HTML input amounts
      if (
        income == null || age == null || creditScore == null || income.toString.length == 0 || age.toString.length == 0 || creditScore.toString.length == 0 || typeof income != "number" || typeof age != "number" || typeof creditScore != "number" || age < 1 || age > 100 || creditScore < 0 || creditScore > 100 || income <= 0 || income > 10000
      ) {
        this.result = "Invalid input types";
      } else {
        let loanAmount = (65 - age) * income * 12 * 0.5; // 50% of all possible earnings

        if (age < 18)
          shouldLoan = "Too young";

        if (age > 64)
          shouldLoan = "Too old";

        if (income < 400)
          shouldLoan = "Too low income";

        if (creditScore > acceptableRisk)
          shouldLoan = "Too spicy client";

        if (shouldLoan == "")
          shouldLoan = "Best offer, sir: " + Math.round(loanAmount * ( 1 + creditScore / 100 ), 0) + " euros for " + (65 - age) + " years";
        else
          shouldLoan = "No money: " + shouldLoan;

        this.result = shouldLoan;
      }
    }
  }
}
</script>

<style>
.form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  background-color: #222;
  padding: 2rem;
  border-radius: 8px;
}

.input-field {
  width: 100%;
  padding: 0.5rem;
  font-size: 1.2rem;
  background-color: #333;
  color: #fff;
  border: 1px solid #555;
  border-radius: 4px;
  min-width: 400px;
}

.submit-button {
  padding: 0.75rem;
  font-size: 1.2rem;
  background-color: #444;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.submit-button:hover {
  background-color: #555;
}

.result {
  font-size: 1.5rem;
  color: #fff;
}
</style>



