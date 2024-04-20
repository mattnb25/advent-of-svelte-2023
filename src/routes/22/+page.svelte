<script>
    let name = "";
</script>

<h1>Day 22 - Snowflake Silhouettes</h1>
<label for="name">Enter your name below to create a customized snowflake.</label
>
<input type="text" id="name" bind:value={name} placeholder="Name" />

<svg viewBox="0 0 100 100" width="10rem" height="10rem" role="img">
    <title>A Snowflake</title>

    <g class="snowflake">
        <g id="tree">
            <g id="branches">
                <line x1="50" y1="50" x2="50" y2="10" class="trunk" />s
                <!-- Start 10 above center. Move 5 left and 5 up -->
                <line x1="50" y1="40" x2="45" y2="35" class="branch" />
                <!-- Start 20 above center. Move 7.5 left and 7.5 up -->
                <line x1="50" y1="30" x2="42.5" y2="22.5" class="branch" />
                <!-- Start 30 above center. Move 10 left and 10 up -->
                <line x1="50" y1="20" x2="40" y2="10" class="branch" />
            </g>
            <use href="#branches" class="flipped-branches" />
        </g>
    </g>
    <!-- intentionally simple due to time, snowflake variablility depends only on name length -->
    {#each Array(name.length) as _, i}
        <!-- divide 360 by length to get a base ratio, then use index as a multiplier -->
        <use
            href="#tree"
            style="transform: rotate({(360 / name.length) * (i + 1)}deg);"
            class="rotated-tree"
        />
        <!-- Duplicate tree only for a thicker density aesthetic -->
        <use
            href="#tree"
            style="transform: rotate({(360 / name.length) * (i + 1.5)}deg);"
            class="rotated-tree"
        />
    {/each}
</svg>

<style>
    input {
        margin: 1rem 0;
        display: block;
    }
    svg {
        display: block;
        margin: 1rem auto;
    }
    line {
        stroke: #000;
    }
    .flipped-branches {
        scale: -1 1;
        transform-origin: center;
    }
    /* rotate might not work on all browsers */
    .rotated-tree {
        transform-origin: center;
    }
</style>
