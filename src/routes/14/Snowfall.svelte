<script>
    export let speed = 50;
    export let quantity = 50;

    $: snowflakes = Array.from({ length: quantity }, () => ({
        x: Math.random() * 100,
        y: Math.random() * 100,
    }));
</script>

<div class="wrapper">
    <div class="snowflakes" style="animation-duration: {100 - speed}s">
        {#each snowflakes as snowflake}
            <span
                class="snowflake"
                style="
                bottom: {snowflake.y}%; 
                left: {snowflake.x}%;
                "
            ></span>
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
        overflow: hidden;
    }

    @keyframes scroll {
        0% {
            transform: translateY(0);
        }
        100% {
            transform: translateY(100%);
        }
    }
    .snowflakes {
        position: relative;
        height: 100dvh;
        width: 100dvw;
        animation: scroll linear infinite;
    }
    .snowflake {
        position: absolute;
        background-color: black;
        height: 0.25rem;
        width: 0.25rem;
    }
</style>
