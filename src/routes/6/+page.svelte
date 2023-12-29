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
<div>
    <button class="tap" on:click={tap}>
        <span>(TAP)</span>
        {bpm} BPM
    </button>
    <button class="reset" on:click={() => (bpm = 0)}>Reset</button>
</div>

<style>
    div {
        position: relative;
        height: 15rem;
        width: 15rem;
    }
    .tap {
        height: 100%;
        width: 100%;
        border-radius: 50%;
        font-size: 2rem;
        transition:
            transform,
            opacity 100ms;
    }
    .tap:active {
        transform: scale(0.95);
        opacity: 0.25;
    }
    .tap span {
        font-size: 1rem;
        display: block;
    }
    .reset {
        position: absolute;
        top: 75%;
        left: 50%;
        transform: translateX(-50%);
    }
</style>
