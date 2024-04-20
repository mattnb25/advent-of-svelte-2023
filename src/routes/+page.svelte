<h1>Advent of Svelte 2023</h1>

<div class="links">
    <a href="https://advent.sveltesociety.dev/" target="_blank">
        The Challenges
    </a>
    <a href="https://github.com/mattnb25/advent-of-svelte-2023" target="_blank">
        Source (GitHub)
    </a>
</div>

<div class="calendar">
    {#each Array(5) as _}
        <span aria-hidden="true"></span>
    {/each}
    {#each Array(24) as _, i}
        <a href="./{i + 1}">{i + 1}</a>
    {/each}
    {#each Array(7) as _, i}
        <span aria-hidden="true">{i + 25}</span>
    {/each}
</div>

<style>
    h1 {
        text-align: center;
    }
    .links {
        display: flex;
        gap: 1rem;
        justify-content: center;
        padding-bottom: 1rem;
    }
    .calendar {
        max-width: 25rem;
        margin: auto;
        display: grid;
        grid-template-columns: repeat(7, 1fr);
    }
    .calendar > * {
        display: flex;
        justify-content: center;
        align-items: center;
        aspect-ratio: 1/1;
    }
    a {
        color: #333;
    }
    span {
        color: #ccc;
    }
</style>
