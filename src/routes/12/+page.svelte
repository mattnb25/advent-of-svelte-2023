<script>
    let imageURLs = [];

    for (let i = 1; i <= 11; i++) {
        imageURLs.push(
            `https://advent.sveltesociety.dev/data/2023/day-twelve/${i}.png`,
        );
    }

    // the current selected image during drag-and-drop
    let selectedImage;

    function dragStart(event) {
        // copy the dragged image to avoid removing it from the palette
        selectedImage = event.target.cloneNode(true);
    }

    function drop(event) {
        // required to prevent default iOS behaviour
        event.preventDefault();

        if (selectedImage) {
            // mouse coordinates relative to the card's top-left corner
            const cardOffsetX =
                event.clientX - event.target.getBoundingClientRect().left;
            const cardOffsetY =
                event.clientY - event.target.getBoundingClientRect().top;

            // dimensions of the card
            const cardWidth = event.target.offsetWidth;
            const cardHeight = event.target.offsetHeight;

            // calculate and set position using percentages
            selectedImage.style.left = `${(cardOffsetX / cardWidth) * 100}%`;
            selectedImage.style.top = `${(cardOffsetY / cardHeight) * 100}%`;

            event.target.appendChild(selectedImage);
            selectedImage = null;
        }
    }
</script>

<h1>Day 12 - Greetings and salutations</h1>

<h2>Palette</h2>
<p>(on mobile devices tap and hold to drag)</p>
<div role="listbox" class="palette">
    <!-- drag might not work on Chrome on Android -->
    {#each imageURLs as url}
        <img src={url} alt="" draggable="true" on:dragstart={dragStart} />
    {/each}
</div>

<h2>Card</h2>
<p>Drag items from the palette onto this area to create a personalized card.</p>
<div
    role="presentation"
    class="card"
    on:dragover|preventDefault
    on:drop={drop}
></div>

<style>
    .palette {
        display: flex;
        gap: 1rem;
        overflow-x: auto;
    }
    .palette img {
        height: 2.5rem;
        margin-bottom: 1rem;
    }

    .card {
        position: relative;
        border: 1px solid black;
        border-radius: 0.5rem;
        box-shadow: 0 0 1rem 0.1rem #3333;
        height: 25rem;
    }
    :global(.card > img) {
        height: 2.5rem;
        position: absolute;
        transform: translate(-50%, -50%);
    }
</style>
