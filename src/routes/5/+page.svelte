<script>
    import { onMount } from "svelte";

    // all elf data and overall progress
    let elves = [];
    let overview = {
        wrapTotal: 0,
        giftTotal: 0,
        wrapTime: 0,
        makeTime: 0,
    };

    onMount(async () => {
        await fetch("https://advent.sveltesociety.dev/data/2023/day-five.json")
            .then((res) => res.json())
            .then((data) => {
                processElves(data);
                processOverview(data);
            });
    });

    // aggregate elf data by individual elves
    function processElves(data) {
        data.forEach((item) => {
            const { elf, task, minutesTaken } = item;

            // create an individual overview for the elf for the first time
            if (!elves[elf]) {
                elves[elf] = {
                    wrapTotal: 0,
                    giftTotal: 0,
                    wrapTime: 0,
                    makeTime: 0,
                };
            }

            if (task === "WRAPPED_PRESENT") {
                elves[elf].wrapTotal++;
                elves[elf].wrapTime += minutesTaken;
            } else if (task === "CREATED_TOY") {
                elves[elf].giftTotal++;
                elves[elf].makeTime += minutesTaken;
            }
        });
    }

    // aggregate elf data by all elves as a whole
    function processOverview(data) {
        data.forEach((item) => {
            const { task, minutesTaken } = item;

            if (task === "WRAPPED_PRESENT") {
                overview.wrapTotal++;
                overview.wrapTime += minutesTaken;
            } else if (task === "CREATED_TOY") {
                overview.giftTotal++;
                overview.makeTime += minutesTaken;
            }
        });
    }
</script>

<h1>Day 5 - Present Progress</h1>
<p>This page shows statistics for Santa's elves.</p>

<h2>Overall Statistics</h2>
<div class="overview">
    <p>Gifts created: {overview.wrapTotal}</p>
    <p>Gifts wrapped: {overview.giftTotal}</p>
    <p>
        Average wrap time:
        {(overview.wrapTime / overview.wrapTotal).toFixed(2)} minutes
    </p>
    <p>
        Average build time:
        {(overview.makeTime / overview.giftTotal).toFixed(2)} minutes
    </p>
</div>

<h2>Personal Statistics</h2>
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Gifts wrapped</th>
            <th>Gifts built</th>
            <th>Average wrap time</th>
            <th>Average build time</th>
        </tr>
    </thead>

    <tbody>
        {#each Object.entries(elves) as [name, elf]}
            <tr>
                <td>{name}</td>
                <td>{elf.wrapTotal}</td>
                <td>{elf.giftTotal}</td>
                <td>{(elf.wrapTime / elf.wrapTotal).toFixed(2)}m</td>
                <td>{(elf.makeTime / elf.giftTotal).toFixed(2)}m</td>
            </tr>
        {/each}
    </tbody>
</table>

<style>
    .overview p {
        margin: 0;
    }
    table {
        display: block;
        padding: 0.5rem;

        border: 1px solid #333;
        border-radius: 0.5rem;
        overflow-x: auto;
    }
    th {
        text-align: start;
        padding-right: 2.5rem;
    }
</style>
