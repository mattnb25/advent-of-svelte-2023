<script>
    import { onMount } from "svelte";
    let nicelist = [];
    let naughtylist = [];

    onMount(async () => {
        const list = await fetch(
            "https://advent.sveltesociety.dev/data/2023/day-one.json",
        ).then((x) => x.json());

        list.forEach((child) => determineBehaviour(child));
    });

    function determineBehaviour(child) {
        child.tally > 0
            ? (nicelist = [...nicelist, child])
            : (naughtylist = [...naughtylist, child]);
    }

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

<div>
    <section>
        <h2>Nice 😇</h2>
        <ul>
            {#each nicelist as child}
                <li>{child.name} {child.tally}</li>
            {/each}
        </ul>
    </section>
    <section>
        <h2>Naughty 😡</h2>
        <ul>
            {#each naughtylist as child}
                <li>{child.name} {child.tally}</li>
            {/each}
        </ul>
    </section>
</div>

<style>
    form,
    div {
        display: flex;
        justify-content: space-evenly;
    }
    input {
        margin: 0.25rem;
        min-width: 5rem;
        width: 100%;
    }
    input[type="submit"] {
        width: 50%;
    }

    section {
        flex: 1;
    }
    section ul {
        list-style: none;
        padding: 0;
    }
</style>
