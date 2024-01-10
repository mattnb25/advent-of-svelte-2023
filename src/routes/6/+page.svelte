<script>
    let bpm = 0;
    let lastTapTime = 0;

    // calculate BPM when the tap button is clicked
    function tap() {
        const currentTapTime = Date.now();
        bpm = Math.round(60000 / (currentTapTime - lastTapTime));
        lastTapTime = currentTapTime;
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
        font-size: xx-large;
    }
    .tap span {
        font-size: 1rem;
    }
</style>
