<script>
    import { onMount } from "svelte";

    // 2D array, 1st dimension is y, 2nd is x, x values are initialized to empty arrays
    let hangar = Array.from({ length: 21 }, () =>
        Array.from({ length: 21 }, () => []),
    );

    let currentCell; // stores the currently selected cell

    onMount(() => {
        fetch("https://advent.sveltesociety.dev/data/2023/day-sixteen.json")
            .then((res) => res.json())
            .then((res) => {
                res.forEach((gift) => {
                    const { x, y } = gift;
                    hangar[y][x].push(gift); // store the object based on its coordinates
                    hangar = hangar; // trigger reactivity
                });
            });
    });
</script>

<h1>Day 16 - Lessons from Amazon ™️</h1>

<h2>Cell Content</h2>
{#if !currentCell || currentCell.length === 0}
    <p>No gifts found.</p>
{:else}
    {#each currentCell as gift}
        <p>X{gift.x} Y{gift.y} {gift.name}</p>
    {/each}
{/if}

<h2>Hangar</h2>
<p>Click on a cell to inspect its contents.</p>
<div class="hangar">
    {#each hangar as row}
        {#each row as cell}
            <span
                role="button"
                tabindex="0"
                class="cell"
                on:keydown={() => (currentCell = cell)}
                on:click={() => (currentCell = cell)}
            ></span>
        {/each}
    {/each}
</div>

<style>
    .hangar {
        display: grid;
        gap: 0.1rem;
        grid-template-rows: repeat(21, 1fr);
        grid-template-columns: repeat(21, 1fr);

        overflow: auto;
        width: min-content;
        max-width: 100%;
    }
    .cell {
        cursor: pointer;
        height: 1.5rem;
        width: 1.5rem;
        border-radius: 0;
        background-color: royalblue;
    }
    .cell:hover {
        background-color: aliceblue;
    }
</style>
