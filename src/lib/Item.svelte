<script>
    export let item;
    export let description;
    export let price;
    export let img;
</script>

<main>
    <h2>{item}</h2>
    <h3>{description}</h3>
    <p>${price}</p>
    <img src={img} alt="" />
</main>

<style>
    img {
        width: 200px;
    }
</style>
