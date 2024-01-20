<script>
    let name = "";
</script>

<h1>Day 22 - Snowflake Silhouettes</h1>
<label for="name">Enter your name below to create a customized snowflake.</label
>
<input type="text" id="name" bind:value={name} placeholder="Name" />

<svg
    style="--tree-count: {name.length}"
    viewBox="0 0 100 100"
    width="10rem"
    height="10rem"
    role="img"
>
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
        <use href="#tree" style="--index: {i + 1};" class="rotated-tree" />
        <!-- Duplicate tree only for a thicker density aesthetic -->
        <use href="#tree" style="--index: {i + 1.5};" class="rotated-tree" />
    {/each}
</svg>

<style>
    input {
        margin: 1rem 0;
        display: block;
    }
    svg {
        margin: 1rem 0;
    }
    line {
        stroke: #000;
    }
    .flipped-branches {
        scale: -1 1;
        transform-origin: center;
    }
    /* rotate might not work in safari */
    .rotated-tree {
        rotate: calc(360deg / var(--tree-count) * var(--index));
        transform-origin: center;
    }
</style>
