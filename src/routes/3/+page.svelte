<script>
    import { onMount } from "svelte";
    let warehouse = [];
    let sleigh = [];
    let warehouseWeight = 0;
    let sleighWeight = 0;

    onMount(async () => {
        warehouse = await fetch(
            "https://advent.sveltesociety.dev/data/2023/day-three.json",
        ).then((x) => x.json());
        warehouse.forEach((gift) => {
            warehouseWeight += gift.weight;
        });
    });

    function load(index, weight) {
        sleigh = [...sleigh, warehouse[index]];
        warehouse.splice(index, 1);
        warehouse = warehouse;
        sleighWeight += weight;
        warehouseWeight -= weight;
    }

    function unload(index, weight) {
        warehouse = [...warehouse, sleigh[index]];
        sleigh.splice(index, 1);
        sleigh = sleigh;
        warehouseWeight += weight;
        sleighWeight -= weight;
    }

    function send() {
        if (sleighWeight > 100) {
            console.log("Sleigh weight exceeded: ", sleighWeight);
            return;
        }
        sleigh = [];
        sleighWeight = 0;
    }
</script>

<h1>Day 3 - Jingle Bell Balancer</h1>

{#if sleighWeight < 100}
    <button type="submit" on:click={send}>Send Gifts 🦌🛷</button>
{:else}
    <p>Remove weight to continue (100kg limit)</p>
{/if}

<div class="sleigh">
    <h2>Sleigh: {sleighWeight.toFixed(2)}kg</h2>
    {#each sleigh as gift, index}
        <button on:click={() => unload(index, gift.weight)}>
            {gift.name}
            {gift.weight}
        </button>
    {/each}
</div>

<div class="warehouse">
    <h2>Warehouse: {warehouseWeight.toFixed(2)}kg</h2>
    {#each warehouse as gift, index}
        <button on:click={() => load(index, gift.weight)}>
            {gift.name}
            {gift.weight}
        </button>
    {/each}
</div>
