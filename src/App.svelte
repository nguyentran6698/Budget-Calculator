<!-- JS -->
<script>
  import { setContext, onMount, afterUpdate } from "svelte";
  // components
  import ExpenseForm from "./ExpenseForm.svelte";
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Total from "./Total.svelte";
  import Modal from "./Modal.svelte";
  import GithubAwait from "./GithubAwait.svelte";
  //data
  // import expensesData from "./expenses";
  // variable
  let expenses = [];
  let setID = null;
  let setName = "";
  let setAmount = null;
  // toggle form variables
  let isFormOpen = false;
  //reactive
  $: total = expenses.reduce((accumulate, current) => {
    return (accumulate += current.amount);
  }, 0);

  $: isEditing = setID ? true : false;
  // function
  function showForm() {
    isFormOpen = true;
  }
  function hideForm() {
    isFormOpen = false;
    setName = "";
    setID = null;
    setAmount = null;
  }
  function removeExpense(id) {
    expenses = expenses.filter((expense) => expense.id !== id);
  }
  function clearExpenses() {
    expenses = [];
  }
  function addExpense({ name, amount }) {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  }
  function editExpense({ name, amount }) {
    expenses = expenses.map((expense) => {
      return expense.id === setID
        ? { ...expense, name, amount }
        : { ...expense };
    });
    setID = null;
    setAmount = null;
    setName = "";
  }
  function setModifiedExpense(id) {
    let expense = expenses.find((expense) => expense.id === id);
    setID = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }
  //context
  setContext("add", addExpense);
  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);
  //local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });
  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<Navbar {showForm} />

<main class="content">
  <GithubAwait />
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {isEditing}
        name={setName}
        amount={setAmount}
        {editExpense}
        {hideForm}
      /></Modal
    >
  {/if}

  <Total title="total expense" {total} />
  <ExpensesList {expenses} />
  <button
    class="btn btn-primary btn-block"
    type="button"
    on:click={clearExpenses}>clear expenses</button
  >
</main>
