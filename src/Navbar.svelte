<script>
  export let showForm;
</script>

<!-- Html -->
<nav class="nav">
  <div class="nav-center">
    <h1 class="nav-title">budget calculator</h1>
    <button class="nav-btn" type="button" on:click={() => showForm()}>
      <i class="far fa-plus-square" />
      add item
    </button>
  </div>
</nav>
