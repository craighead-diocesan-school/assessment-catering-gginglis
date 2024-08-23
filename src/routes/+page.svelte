<script>
  import Header from "$lib/Header.svelte";
  import Item from "$lib/Item.svelte";

  async function getMenu() {
    let menuData = await fetch(
      "https://digitech.craighead.school.nz/api/restaurant",
    );
    return menuData.json();
  }
  let menu = getMenu();

  let custom = [];

  function addToMenu(item) {
    custom = [...custom, item];
  }
</script>

<head>
  <style>
    @import url("https://fonts.googleapis.com/css2?family=Anton+SC&family=Urbanist:ital,wght@0,100..900;1,100..900&display=swap");
  </style>
</head>

<Header />

<main>
  <div class="columns">
    <div class="column">
      <h2>Menu Options</h2>
      {#await menu}
        loading
      {:then menu}
        {#each menu.breakfast as breakfast}
          <Item {...breakfast} />
          <button on:click={() => addToMenu()}>add to menu</button>
        {/each}
        {#each menu.dinner as dinner}
          <Item {...dinner} />
          <button on:click={() => addToMenu()}>add to menu</button>
        {/each}
        {#each menu.dessert as dessert}
          <Item {...dessert} />
          <button on:click={() => addToMenu()}>add to menu</button>
        {/each}
      {/await}
    </div>
    <div class="column">
      <h2><input type="text" /></h2>
    </div>
  </div>
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>

<style>
</style>
