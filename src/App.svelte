<!-- JS -->
<script>
  import { setContext } from "svelte";
  // components
  import ExpenseForm from "./ExpenseForm.svelte";
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Total from "./Total.svelte";
  //data
  import expensesData from "./expenses";
  // variable
  let expenses = [...expensesData];
  //reactive
  $: total = expenses.reduce((accumulate, current) => {
    return (accumulate += current.amount);
  }, 0);
  // function
  function removeExpense(id) {
    expenses = expenses.filter((expense) => expense.id !== id);
  }
  function clearExpenses() {
    expenses = [];
  }
  function addExpense({ name, amount }) {
    let expense = { id: Math.random * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  }

  //context
  setContext("add", addExpense);
  setContext("remove", removeExpense);
  //
</script>

<Navbar />

<main class="content">
  <ExpenseForm />
  <Total title="total expense" {total} />
  <ExpensesList {expenses} />
  <button
    class="btn btn-primary btn-block"
    type="button"
    on:click={clearExpenses}>clear expenses</button
  >
</main>
