<script>
  import Header from "$lib/Header.svelte";
  import Item from "$lib/Item.svelte";
  import { onMount } from "svelte";

  async function getMenu() {
    let menuData = await fetch(
      "https://digitech.craighead.school.nz/api/restaurant",
    );
    return menuData.json();
  }
  let menu = getMenu();

  let custom = [];

  function selectedItem() {
    selected = !selected;
  }

  function addToMenu(food) {
    custom = [...custom, food];
    calculateGst;
  }

  function removeFromMenu(index) {
    custom = [...custom.slice(0, index), ...custom.slice(index + 1)];
  }

  let gst = 0.15;

  function calculateGst() {
    gst = 0;
    for (let gst of food) {
      gst = food.price * gst;
    }
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
          <div class:highlight={breakfast.selected}>
            <Item {...breakfast} />
            <button
              on:click={() => addToMenu(breakfast)}
              disabled={custom.includes(breakfast)}>add to menu</button
            >
          </div>
        {/each}

        {#each menu.dinner as dinner}
          <Item {...dinner} />
          <button
            on:click={() => addToMenu(dinner)}
            disabled={custom.includes(dinner)}>add to menu</button
          >
        {/each}
        {#each menu.dessert as dessert}
          <Item {...dessert} />
          <button
            on:click={() => addToMenu(dessert)}
            disabled={custom.includes(dessert)}>add to menu</button
          >
        {/each}
      {/await}
    </div>
    <div class="column">
      <h2>custom menu</h2>
      {#if custom.length >= 1}
        {#each custom as chosen, index}
          <p>${gst}</p>
          <Item {...chosen} />
          <button
            on:click={() => {
              removeFromMenu(index);
            }}>remove</button
          >
        {/each}
      {:else}
        <p>menu has no items</p>
      {/if}
    </div>
  </div>
</main>

<footer>
  <p>&copy; Craighead Diocesan School 2024</p>
</footer>

<style>
  h2 {
    font-family: "Anton SC", sans-serif;
    font-size: 2.4rem;
    font-weight: 500;
    margin: 30px;
    color: rgb(147, 175, 147);
    text-shadow: 0.7px 0.7px rgb(82, 118, 82);
  }
  .highlight {
    background-color: rgb(147, 175, 147);
  }

  p {
    font-family: "Urbanist", sans-serif;
    font-weight: 400;
    font-size: 1.1rem;
  }
</style>
