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
  // starts the menu as an empty array

  let gst = 0.15;

  let customName = [];

  function addToMenu(food) {
    custom = [...custom, food];
    food.selected = true;
    menu = menu;
    // adds the chosen object to the custom menu and highlights it
  }

  function removeFromMenu(index, food) {
    custom = [...custom.slice(0, index), ...custom.slice(index + 1)];
    food.selected = false;
    menu = menu;
    // removes the item from the custom menu and returns it to its original visibility
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

      <!-- waits for the menu item to load, then displays the items invidually once loaded -->
      {#await menu}
        loading
        <!-- displays loading message while data is being recieved from the external source -->
      {:then menu}
        {#each menu.breakfast as breakfast}
          <div class:highlight={breakfast.selected}>
            <!-- only highlights items that are in the custom menu -->
            <Item {...breakfast} />
            <button
              on:click={() => addToMenu(breakfast)}
              disabled={custom.includes(breakfast)}
            >
              <!-- adds the item to the menu, and disables the button when the item is already present -->
              add to menu
            </button>
          </div>
        {/each}

        {#each menu.dinner as dinner}
          <div class:highlight={dinner.selected}>
            <Item {...dinner} />
            <button
              on:click={() => addToMenu(dinner)}
              disabled={custom.includes(dinner)}>add to menu</button
            >
          </div>
        {/each}
        {#each menu.dessert as dessert}
          <div class:highlight={dessert.selected}>
            <Item {...dessert} />
            <button
              on:click={() => addToMenu(dessert)}
              disabled={custom.includes(dessert)}>add to menu</button
            >
          </div>
        {/each}
      {/await}
    </div>
    <div class="column">
      <h2>{customName}</h2>
      <h3>
        <input
          type="text"
          bind:value={customName}
          placeholder="enter menu name"
        />
      </h3>
      <!-- displays items chosen for custom menu with their respective gst amount, and allows the user to remove items if they change their mind -->
      {#if custom.length > 0}
        {#each custom as food, index}
          <Item
            item={food.item}
            description={food.description}
            price={`${food.price} + $${(food.price * gst).toFixed(2)} GST`}
            img={food.img}
          />
          <button
            on:click={() => {
              removeFromMenu(index, food);
            }}>remove</button
          >
        {/each}
      {:else}
        <p>menu has no items</p>
        <!-- displays a message when the custom menu has no items -->
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
    background-color: rgba(147, 175, 147, 0.397);
    border-radius: 30px;
    transition: 0.25s;
  }

  p {
    font-family: "Urbanist", sans-serif;
    font-weight: 400;
    font-size: 1.1rem;
  }

  button {
    font-family: "Urbanist", sans-serif;
    font-weight: 500;
    font-size: 1.1rem;
    background-color: rgb(147, 175, 147);
    color: rgb(255, 255, 255);
    text-shadow: 0.7px 0.7px rgb(82, 118, 82);
    border: solid rgb(111, 144, 111) 2px;
    border-radius: 10px;
    margin: 0px 20px 20px;
    transition: 0.25s;
    padding: 3px 10px;
  }

  button:hover {
    background-color: rgb(111, 144, 111);
    transition: 0.2s;
  }

  button:disabled {
    color: rgba(255, 255, 255, 0.565);
    background-color: rgba(111, 144, 111, 0.353);
    border: solid rgb(111, 144, 111, 0.353) 2px;
    text-shadow: 0.7px 0.7px rgba(82, 118, 82, 0.733);
    transition: 0.2s;
  }

  h3 {
    margin-top: 30px;
  }
</style>
