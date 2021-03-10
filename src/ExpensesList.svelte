<script>
  import { fly, blur } from "svelte/transition";
  import { flip } from "svelte/animate";
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
    {#each expenses as expense, index (expense.id)}
      <div
        in:fly={{ x: 200, delay: (index + 1) * 700 }}
        out:fly={{ x: -200 }}
        animate:flip
      >
        <Expense {...expense} />
      </div>
    {:else}
      <h2 transition:blur={{ delay: 200 }}>No expenses added to the list</h2>
    {/each}
  </ul>
</section>

<style>
  h2 {
    text-transform: capitalize;
    color: red;
  }
</style>
