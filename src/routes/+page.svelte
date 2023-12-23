<h1>Advent of Svelte 2023</h1>

{#each Array(24) as _, i}
    <a href="./{i + 1}">{i + 1}</a>
{/each}

<a href="https://github.com/MNB-87/advent-of-svelte">Source (GitHub)</a>

<style>
    a {
        display: inline-block;
        min-width: 2.5rem;
        min-height: 2.5rem;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        color: black;
    }
</style>
