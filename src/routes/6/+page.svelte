<script>
    let bpm = 0;
    let lastTap = 0;

    function tap() {
        const currentTap = Date.now();
        bpm = Math.round(60000 / (currentTap - lastTap));
        lastTap = currentTap;
    }
</script>

<h1>Day 6 - Misteltoe Metronome</h1>
<button class="tap" on:click={tap}>
    <span>(TAP)</span>
    <br />
    {bpm} BPM
</button>
<button on:click={() => (bpm = 0)}>Reset</button>

<style>
    .tap {
        margin-bottom: 1rem;
        height: 10rem;
        width: 100%;
        font-size: 2rem;
        transition: opacity 100ms;
    }
    .tap:active {
        opacity: 0.25;
    }
    .tap span {
        font-size: 1rem;
    }
</style>
