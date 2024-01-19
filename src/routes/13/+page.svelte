<script>
    import { onMount } from "svelte";

    // the groups of gifts, each should have < 100kg of gifts
    let sleighs = [];

    onMount(async () => {
        await fetch(
            "https://advent.sveltesociety.dev/data/2023/day-thirteen.json",
        )
            .then((data) => data.json())
            .then((data) => {
                // track each load and its weight
                let currentLoad = [];
                let weight = 0;

                data.forEach((gift) => {
                    weight += gift.weight;

                    if (weight > 100) {
                        sleighs = [...sleighs, currentLoad]; // add the current load to the list of sleighs
                        weight = gift.weight; // include the current gift instead of resetting to 0
                        currentLoad = []; // reset current load
                    }

                    // add the gift to the current load
                    currentLoad = [...currentLoad, gift];
                });
            });
    });
</script>

<h1>Day 13 - Jingle Bell Balancer 2.0</h1>
<p>
    Each sleigh here contains less than 100kg of gifts for Santa to carry on a
    single trip.
</p>

{#each sleighs as sleigh, index}
    <h2>Sleigh {index + 1}</h2>
    {#each sleigh as gift}
        <p>{gift.name}: {gift.weight}kg</p>
    {/each}
{/each}

<style>
    p {
        margin: 0;
    }
</style>
