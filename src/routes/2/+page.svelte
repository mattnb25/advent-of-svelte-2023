<script>
    import { fade } from "svelte/transition";
    let cookieCount = 0;
    let eating = false;

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
<h2>
    Santa has eaten {cookieCount}
    {cookieCount === 1 ? "cookie" : "cookies"}
</h2>

<div>
    <button on:click={add}>add</button>
    <button on:click={remove}>remove</button>
    <button on:click={reset}>reset</button>
</div>

<div>
    <span>🎅</span>
    {#if eating}
        <span transition:fade>🍪 CHOMP!</span>
    {/if}
</div>

<style>
    button {
        margin: 0.25rem;
        min-width: 8rem;
        color: #000;
    }

    span {
        display: inline-block;
        padding: 1rem 0;
        font-size: 3.2rem;
    }
</style>
