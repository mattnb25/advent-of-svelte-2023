<script>
    import { onMount } from "svelte";

    // Variables to store gift data and track weights
    let warehouse = [];
    let warehouseWeight = 0;
    let sleigh = [];
    let sleighWeight = 0;

    onMount(async () => {
        await fetch("https://advent.sveltesociety.dev/data/2023/day-three.json")
            .then((data) => data.json())
            .then((data) => {
                // Assign fetched data to the warehouse array
                warehouse = data;

                // Calculate initial warehouse weight
                data.forEach((gift) => {
                    warehouseWeight += gift.weight;
                });
            });
    });

    // move a gift from the warehouse to the sleigh
    function load(index, weight) {
        sleigh = [...sleigh, warehouse[index]];
        warehouse.splice(index, 1);
        warehouse = warehouse;

        sleighWeight += weight;
        warehouseWeight -= weight;
    }

    // move a gift from the sleigh to the warehouse
    function unload(index, weight) {
        warehouse = [...warehouse, sleigh[index]];
        sleigh.splice(index, 1);
        sleigh = sleigh;

        warehouseWeight += weight;
        sleighWeight -= weight;
    }

    // reset the sleigh and its weight
    function send() {
        // enforce the weight limit
        if (sleighWeight > 100) return;

        sleigh = [];
        sleighWeight = 0;
    }
</script>

<h1>Day 3 - Jingle Bell Balancer</h1>

{#if sleighWeight < 100}
    <button class="sleigh-button" on:click={send}> Send Gifts 🦌🛷 </button>
{:else}
    <p>Remove weight to continue (100kg limit)</p>
{/if}

<div class="sleigh">
    <h2>Sleigh: {sleighWeight.toFixed(2)}kg</h2>
    <p>Click items to remove them from the sleigh.</p>

    {#each sleigh as gift, index}
        <button on:click={() => unload(index, gift.weight)}>
            {gift.name}
            {gift.weight}
        </button>
    {/each}
</div>

<div class="warehouse">
    <h2>Warehouse: {warehouseWeight.toFixed(2)}kg</h2>
    <p>Click items to add them to the sleigh.</p>

    {#each warehouse as gift, index}
        <button on:click={() => load(index, gift.weight)}>
            {gift.name}
            {gift.weight}
        </button>
    {/each}
</div>

{#if warehouse.length === 0}
    <p><i> The sleigh is currently empty.</i></p>
{/if}

<style>
    .sleigh-button {
        font-size: large;
    }
</style>
