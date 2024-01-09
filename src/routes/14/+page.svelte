<script>
    import Snowfall from "./Snowfall.svelte";

    let speed, quantity;
</script>

<h1>Day 14 - Snowfall Symphony</h1>

<label for="speed">Speed</label>
<input type="range" id="speed" bind:value={speed} min="1" max="99" />

<label for="quantity">Quantity</label>
<input type="range" id="quantity" bind:value={quantity} min="1" max="99" />

<Snowfall {speed} {quantity}></Snowfall>

<style>
    input {
        margin-bottom: 1rem;
    }
</style>
