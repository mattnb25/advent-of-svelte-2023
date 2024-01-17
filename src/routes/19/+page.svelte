<script>
    import { onDestroy } from "svelte";

    // customization options
    let colors = ["#00FFFF", "#FF00FF", "#FFFF00"];
    let length = 1;
    let speed = 2.5;

    let offset = 0; // track of color index offset
    let interval;

    // recreate interval on variable change
    $: {
        clearInterval(interval);
        interval = setInterval(
            () => {
                offset++;
            },
            1000 - speed * 100,
        );
    }

    onDestroy(() => {
        clearInterval(interval);
    });
</script>

<h1>Day 19 - Polar Lights</h1>

<h2>Colors</h2>
<input type="color" bind:value={colors[0]} />
<input type="color" bind:value={colors[1]} />
<input type="color" bind:value={colors[2]} />

<h2>Section Length</h2>
<input type="range" bind:value={length} min="1" max="5" />

<h2>Movement Speed</h2>
<input type="range" bind:value={speed} min="1" max="5" />

<div class="lights">
    <!-- base lenght -->
    {#each Array(100) as _, i}
        <!-- multiply by color -->
        {#each Array(length) as _}
            <span
                style="
                color: {colors[(i + offset) % colors.length]};
                background-color: currentColor;
                "
            ></span>
        {/each}
    {/each}
</div>

<style>
    span {
        margin: 0.25rem;
        display: inline-block;
        width: 1rem;
        height: 1rem;
        box-shadow: 0 0 1rem 0.1rem;
    }
    .lights {
        padding-top: 2.5rem;
    }
</style>
