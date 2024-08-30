<script>
    export let item;
    export let description;
    export let price;
    export let img;
</script>

<!-- displays each food item as a seperate object -->
<main>
    <h2>{item}</h2>
    <h3>{description}</h3>
    <p>${price}</p>
    <img src={img} alt="" />
</main>

<style>
    img {
        height: 200px;
        margin: 5px;
    }

    h2 {
        font-family: "Urbanist", sans-serif;
        font-weight: 500;
        font-size: 1.6rem;
    }

    h3 {
        font-family: "Urbanist", sans-serif;
        font-weight: 400;
        font-size: 1.3rem;
    }

    p {
        font-family: "Urbanist", sans-serif;
        font-weight: 400;
        font-size: 1.1rem;
    }

    main {
        margin: 20px;
        padding: 10px;
        transition: 0.25s;
    }
</style>
