<script>
  export let expenses = [];
  import Title from "./Title.svelte";
  import Expense from "./Expense.svelte";
  import App from "./App.svelte";
  import Example from "./Example.svelte";
  import Navbar from "./Navbar.svelte";
</script>

<section>
  <Title title="expense list" />
  <ul>
    {#each expenses as expense}
      <Expense {...expense} />
    {:else}
      <h2>No expenses added to the list</h2>
    {/each}
  </ul>
</section>

<style>
  h2 {
    text-transform: capitalize;
    color: red;
  }
</style>
