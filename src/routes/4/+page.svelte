<script>
    import { onDestroy, onMount } from "svelte";

    let allHistory = [];
    $: currentBPM = allHistory[allHistory.length - 1] || 0;

    let minutesHistory;
    let averageBPM;

    onMount(() => {
        const fetchHeartRate = setInterval(async () => {
            const res = await fetch(
                "https://advent.sveltesociety.dev/data/2023/day-four.json",
            ).then((x) => x.json());

            if (allHistory.length >= 3600) allHistory.shift();
            allHistory = [...allHistory, res.heartRate];

            averageBPM = calculateBPM(minutesHistory);
        }, 1000);
        return () => clearInterval(fetchHeartRate);
    });

    function calculateBPM(minutes) {
        let seconds = minutes * 60;

        if (seconds < allHistory.length) {
            let sum = 0;
            for (let i = 0; i < seconds; i++) sum += allHistory[i];
            return sum / seconds;
        } else {
            return 0;
        }
    }
</script>

<h1>Day 4 - Heart of Christmas</h1>
<p>Current BPM: {currentBPM.toFixed(2)}</p>
<input
    type="number"
    bind:value={minutesHistory}
    placeholder="Type here to get an average"
    min="1"
    max="60"
/>
{#if minutesHistory}
    <p>
        {minutesHistory} minute average:
        {#if averageBPM == 0}
            please wait {minutesHistory * 60 - allHistory.length} seconds
        {:else}
            {averageBPM.toFixed(2)}
        {/if}
    </p>
{/if}

<style>
    input {
        min-width: 15rem;
    }
</style>
