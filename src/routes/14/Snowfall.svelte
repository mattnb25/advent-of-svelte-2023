<script>
    export let speed = 50; // snowfall speed
    export let quantity = 50; // snowflake quantity

    // collection of snowflakes with random positions
    $: snowflakes = Array.from({ length: quantity }, () => ({
        x: Math.random() * 100,
        y: Math.random() * 100,
    }));
</script>

<!-- wrapper that covers the entire viewport -->
<div class="wrapper">
    <!-- contains a layer of snowflakes twice the height of the viewport -->
    <div class="snowflakes" style="animation-duration: {100 - speed}s">
        {#each snowflakes as snowflake}
            <!-- create initial snowflakes -->
            <span
                class="snowflake"
                style="
                bottom: {snowflake.y}%; 
                left: {snowflake.x}%;
                "
            ></span>
            <!-- duplicate initial snowflakes to appear when scrolled down-->
            <span
                class="snowflake"
                style="
                bottom: {snowflake.y + 100}%; 
                left: {snowflake.x}%;
                "
            ></span>
        {/each}
    </div>
</div>

<style>
    .wrapper {
        position: absolute;
        z-index: -1;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        overflow: hidden;
    }

    @keyframes scroll {
        0% {
            transform: translateY(0%);
        }
        100% {
            transform: translateY(100%);
        }
    }
    .snowflakes {
        position: relative;
        height: 100%;
        animation: scroll linear infinite;
    }
    .snowflake {
        position: absolute;
        background-color: white;
        box-shadow: 0 0 0.5rem 0.1rem #333a;
        height: 0.25rem;
        width: 0.25rem;
        border-radius: 0.5rem;
    }
</style>
