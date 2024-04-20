<script>
    // card objects, selected card objects, and game start time in ms
    let cards = [];
    let selection = [];
    const startTime = Date.now();

    // create and shuffle cards
    (() => {
        // create 24 cards
        for (let i = 1; i <= 24; i++) {
            // create matching pairs
            for (let j = 0; j < 2; j++) {
                cards.push({
                    id: i,
                    img: `https://advent.sveltesociety.dev/data/2023/day-eight/${i}.png`,
                    selected: false,
                    matched: false,
                });
            }
        }
        // shuffle
        cards = cards.sort(() => Math.random() - 0.5);
    })();

    // handle card selection
    function select(card) {
        // reset previous selections
        if (selection.length >= 2) {
            selection[0].selected = false;
            selection[1].selected = false;
            selection = [];
        }

        // mark the current card as selected
        card.selected = true;
        cards = cards;
        selection = [...selection, card];

        // only check the match if the selection contains 2 distinct cards
        if (selection.length === 2 && selection[0] !== selection[1]) {
            checkMatch();
        }
    }

    // check if selected cards match
    function checkMatch() {
        if (selection[0].id === selection[1].id) {
            selection[0].matched = true;
            selection[1].matched = true;
        }
    }
</script>

<h1>Day 8: Santa's Mysterious Deck of Doubles</h1>

{#if cards.every((card) => card.matched)}
    <h2>
        You won! Final time:
        {Math.trunc((Date.now() - startTime) / 1000)}
        seconds
    </h2>
{:else}
    <p>Match the cards:</p>
{/if}

<div class="grid">
    {#each cards as card}
        <button on:click={() => select(card)} disabled={card.matched}>
            <img
                src={card.img}
                loading="lazy"
                alt="card"
                style:filter={card.selected || card.matched
                    ? "none"
                    : "brightness(0)"}
            />
        </button>
    {/each}
</div>

<style>
    .grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(5rem, 1fr));
    }
    button {
        background: none;
        border: none;
    }
    img {
        width: 100%;
        cursor: pointer;
        transition: filter 0.2s ease-in-out;
    }
</style>
