<script>
    import { onMount } from "svelte";

    // BPM history in seconds and latest reading
    let BPMHistory = [];
    $: BPMlatest = BPMHistory[BPMHistory.length - 1] || 0;

    // subset of history in minutes and BPM average based on that value
    let selectedMinutes;
    let averageBPM;

    onMount(() => {
        const interval = setInterval(async () => {
            // fetch heart rate data every second
            const res = await fetch(
                "https://advent.sveltesociety.dev/data/2023/day-four.json",
            ).then((x) => x.json());

            // Keep only the last hour of data
            if (BPMHistory.length >= 3600) BPMHistory.shift();
            BPMHistory = [...BPMHistory, res.heartRate];

            // Update the average BPM with new data
            averageBPM = calculateBPM(selectedMinutes);
        }, 1000);

        return () => clearInterval(interval);
    });

    function calculateBPM(minutes) {
        let seconds = minutes * 60;

        // ensure enough data for the calculation
        if (seconds > BPMHistory.length) return null;

        // calculate the average BPM
        let sum = 0;
        for (let i = 0; i < seconds; i++) sum += BPMHistory[i];
        return sum / seconds;
    }
</script>

<h1>Day 4 - Heart of Christmas</h1>

<p>
    Santa's current BPM: {BPMlatest.toFixed() <= 0
        ? "loading..."
        : BPMlatest.toFixed()}
</p>
<input
    type="number"
    bind:value={selectedMinutes}
    placeholder="Input minutes here to get an average BPM"
    min="1"
    max="60"
/>

{#if selectedMinutes}
    <p>
        {selectedMinutes} minute average:
        {#if !averageBPM}
            please wait {selectedMinutes * 60 - BPMHistory.length} seconds
        {:else}
            {averageBPM.toFixed()}
        {/if}
    </p>
{/if}

<style>
    input {
        min-width: 100%;
    }
</style>
