<script>
    import { fade } from "svelte/transition";
    let cookieCount = 0;
    let eating;

    function add() {
        cookieCount++;
        eating = true;
        setTimeout(() => {
            eating = false;
        }, 500);
    }
    function remove() {
        cookieCount--;
    }
    function reset() {
        cookieCount = 0;
    }
</script>

<h1>Day 2 - Merry Munch-o-Meter</h1>

<div>
    <span>🎅</span>
    {#if eating}
        <span transition:fade>🍪</span>
    {/if}
</div>

<p>
    Santa has eaten {cookieCount}
    {cookieCount === 1 ? "cookie" : "cookies"}
</p>

<div>
    <button on:click={add}>add</button>
    <button on:click={remove}>remove</button>
    <button on:click={reset}>reset</button>
</div>

<style>
    span {
        font-size: xx-large;
    }
</style>
