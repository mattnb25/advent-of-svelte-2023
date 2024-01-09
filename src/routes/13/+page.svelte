<script>
    import { onMount } from "svelte";

    let sleighs = [];

    onMount(async () => {
        const warehouse = await fetch(
            "https://advent.sveltesociety.dev/data/2023/day-thirteen.json",
        ).then((x) => x.json());

        let load = [];
        let weight = 0;

        warehouse.forEach((gift) => {
            weight += gift.weight;

            if (weight > 100) {
                sleighs = [...sleighs, load];
                load = [];
                weight = 0;
            }

            load = [...load, gift];
        });
    });
</script>

<h1>Day 13 - Jingle Bell Balancer 2.0</h1>

{#if sleighs.length === 0}
    <p>loading...</p>
{/if}

{#each sleighs as sleigh, index}
    <h2>Sleigh {index + 1}</h2>
    {#each sleigh as gift}
        <p>{gift.name} - {gift.weight}kg</p>
    {/each}
{/each}

<style>
    p {
        margin: 0;
    }
</style>
