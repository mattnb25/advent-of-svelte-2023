<script>
    import { onMount } from "svelte";

    // lists to store children based on their behavior
    let niceChildren = [];
    let naughtyChildren = [];

    // fetch initial data, parse it, then process it on component mount
    onMount(async () => {
        await fetch("https://advent.sveltesociety.dev/data/2023/day-one.json")
            .then((data) => data.json())
            .then((data) => {
                data.forEach((child) => determineBehaviour(child));
            });
    });

    // assign child to the appropriate list based on tally
    function determineBehaviour(child) {
        child.tally > 0
            ? (niceChildren = [...niceChildren, child])
            : (naughtyChildren = [...naughtyChildren, child]);
    }

    // use form input to add a new child to the appropriate list
    function addChild(e) {
        let newChild = new FormData(e.target);
        determineBehaviour(Object.fromEntries(newChild));
    }
</script>

<h1>Day 1 - Naughty or Nice</h1>

<form on:submit|preventDefault={addChild}>
    <input type="text" name="name" placeholder="Name" required />
    <input type="number" name="tally" placeholder="Tally" required />
    <input type="submit" value="Add" />
</form>

<div class="lists">
    <section>
        <h2>Nice 😇</h2>
        <ul>
            {#each niceChildren as child}
                <li>{child.name} {child.tally}</li>
            {/each}
        </ul>
    </section>
    <section>
        <h2>Naughty 😡</h2>
        <ul>
            {#each naughtyChildren as child}
                <li>{child.name} {child.tally}</li>
            {/each}
        </ul>
    </section>
</div>

{#if niceChildren.length === 0 || naughtyChildren.length === 0}
    <p>loading...</p>
{/if}

<style>
    input {
        display: block;
        margin: 0.5rem 0;
        width: 100%;
    }

    .lists {
        display: flex;
        justify-content: space-evenly;
    }
    section {
        flex: 1;
    }
    section ul {
        list-style: none;
        padding: 0;
    }
</style>
