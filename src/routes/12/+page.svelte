<script>
    let imageURLs = [];

    for (let i = 1; i <= 11; i++) {
        imageURLs.push(
            `https://advent.sveltesociety.dev/data/2023/day-twelve/${i}.png`,
        );
    }

    let selectedImage;

    function dragStart(event) {
        selectedImage = event.target.cloneNode(true);
    }

    function dragOver(event) {
        event.preventDefault();
    }

    function drop(event) {
        event.preventDefault();
        if (selectedImage) {
            const x = event.clientX - event.target.getBoundingClientRect().left;
            const y = event.clientY - event.target.getBoundingClientRect().top;

            selectedImage.style.left = `${x}px`;
            selectedImage.style.top = `${y}px`;

            event.target.appendChild(selectedImage);
            selectedImage = null;
        }
    }
</script>

<h1>Day 12 - Greetings and salutations</h1>
<h2>Palette</h2>
<div role="listbox" class="palette">
    {#each imageURLs as url}
        <img src={url} alt="" draggable="true" on:dragstart={dragStart} />
    {/each}
</div>

<h2>Card</h2>
<div role="presentation" class="card" on:dragover={dragOver} on:drop={drop}>
    <p>Merry Christmas</p>
</div>

<style>
    .palette {
        display: flex;
        gap: 1rem;
        overflow-x: auto;
    }
    .palette img {
        object-fit: contain;
        height: 2.5rem;
        width: 2.5rem;
    }

    .card {
        position: relative;
        border: 1px solid black;
        height: 25rem;
    }
    .card > * {
        position: absolute;
        transform: translate(-50%, -50%);
    }
    .card p {
        font-size: xx-large;
        text-align: center;
        pointer-events: none;
        background-color: #ffffffaa;
        backdrop-filter: blur(2.5px);

        z-index: 1;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -100%);
    }
</style>
