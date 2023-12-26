<script>
    import { onMount } from "svelte";
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

    function processElves(data) {
        data.forEach((item) => {
            const { elf, task, minutesTaken } = item;

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
<div>
    <p>Gifts created: {overview.wrapTotal}</p>
    <p>Gifts wrapped: {overview.giftTotal}</p>
    <p>
        Average wrapping time:
        {(overview.wrapTime / overview.wrapTotal).toFixed(2)}min
    </p>
    <p>
        Average creation time:
        {(overview.makeTime / overview.giftTotal).toFixed(2)}min
    </p>
</div>

<table>
    <thead>
        <tr>
            <th>Elf Name</th>
            <th>Gifts wrapped</th>
            <th>Gifts made</th>
            <th>Average Wrap Time (personal)</th>
            <th>Average Gift Time (personal)</th>
        </tr>
    </thead>

    <tbody>
        {#each Object.entries(elves) as [name, elf]}
            <tr>
                <td>{name}</td>
                <td>{elf.wrapTotal}</td>
                <td>{elf.giftTotal}</td>
                <td>{(elf.wrapTime / elf.wrapTotal).toFixed(2)}min</td>
                <td>{(elf.makeTime / elf.giftTotal).toFixed(2)}min</td>
            </tr>
        {/each}
    </tbody>
</table>

<style>
    table {
        display: block;
        overflow-x: auto;
        position: relative;
    }
    th {
        text-align: start;
        padding: 1rem 2.5rem 1rem 0;
    }
</style>
