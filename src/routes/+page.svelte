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
</script>

<Header />

<main>
  {#await menu}
    loading
  {:then menu}
    {#each menu.breakfast as breakfast}
      <Item {...breakfast} />
    {/each}
    {#each menu.dinner as dinner}
      <Item {...dinner} />
    {/each}
    {#each menu.dessert as dessert}
      <Item {...dessert} />
    {/each}
  {/await}
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>
