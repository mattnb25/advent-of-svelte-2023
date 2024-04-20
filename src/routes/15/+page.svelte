<script>
    import Snowfall from "../14/Snowfall.svelte";

    // reactive values
    let speed, quantity, files, imgSrc;

    function updateImg() {
        // read file as base64 encoded string
        const reader = new FileReader();
        reader.readAsDataURL(files[0]);

        // run when FileReader reading is complete
        reader.onload = (e) => {
            // e.target.result contains the DataURL result
            imgSrc = e.target.result;
        };
    }
</script>

<h1>Day 15 - Sparkling Snowglobes</h1>

<label for="speed">Speed</label>
<input type="range" id="speed" bind:value={speed} min="1" max="99" />

<label for="quantity">Quantity</label>
<input type="range" id="quantity" bind:value={quantity} min="1" max="99" />

<label for="img">Snowglobe background:</label>
<input type="file" id="img" accept="image/*" bind:files on:change={updateImg} />

<div class="globe">
    <img src={imgSrc} alt="" />
    <Snowfall {speed} {quantity}></Snowfall>
</div>

<style>
    label {
        display: block;
    }
    input {
        margin-bottom: 1rem;
    }
    input[type="file"] {
        margin-top: 0.5rem;
    }

    .globe {
        margin: 2rem auto;
        position: relative;
        overflow: hidden;

        transform: scale(1.1);
        border-radius: 50%;
        width: min(100%, 25rem);
        aspect-ratio: 1/1;
        box-shadow: inset 0 0 10px 1px #0003;
    }
    .globe > img {
        position: relative;
        z-index: -2;
        height: 100%;
        width: 100%;
        object-fit: cover;
        filter: brightness(0.8);
    }
</style>
