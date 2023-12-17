<div class="calendar">
    <a class="main-link" href="https://github.com/MNB-87/advent-of-svelte">
        Advent of Svelte 2023
    </a>

    {#each Array(24) as index, i}
        <a class="day" href="./{i + 1}">{i + 1}</a>
    {/each}

    {#each Array(7) as index, i}
        <span class="day extra" aria-hidden="true"> {i + 25}</span>
    {/each}
</div>

<style>
    .main-link {
        font-weight: bold;
        grid-column: 1 / 6;
    }
    a {
        display: flex;
        align-items: center;
        justify-content: center;
        text-decoration: none;
        transition: background-color 1s;
    }
    a:hover {
        background-color: hsla(0, 0%, 100%, 0.25);
    }
    .calendar {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 100%;
        max-width: 40rem;
        max-height: 100dvh;
        display: grid;
        grid-template-columns: repeat(7, 1fr);
    }
    .day {
        aspect-ratio: 1/1;
        display: flex;
        align-items: center;
        justify-content: center;
        border-bottom: 1px solid;
    }
    .extra {
        opacity: 0.25;
    }
</style>
