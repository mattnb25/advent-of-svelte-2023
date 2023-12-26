<script>
    import { onDestroy, onMount } from "svelte";

    let history = [];
    $: currentBPM = history[history.length - 1] || 0;

    let period;
    let averageBPM;

    onMount(() => {
        const fetchHeartRate = setInterval(async () => {
            const res = await fetch(
                "https://advent.sveltesociety.dev/data/2023/day-four.json",
            ).then((x) => x.json());

            if (history.length >= 3600) history.shift();
            history = [...history, res.heartRate];

            averageBPM = calculateBPM(period);
            console.log(history);
        }, 1000);
        return () => clearInterval(fetchHeartRate);
    });

    function calculateBPM(minutes) {
        let seconds = minutes * 60;
        if (seconds < history.length) {
            let sum = 0;
            for (let i = 0; i < seconds; i++) {
                sum += history[i];
            }
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
    bind:value={period}
    placeholder="Type here to get an average"
    min="1"
    max="60"
/>
{#if period}
    <p>
        {period} minute average:
        {#if averageBPM == 0}
            please wait {period * 60 - history.length} seconds
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
