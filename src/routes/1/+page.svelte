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
        determineBehaviour(Object.fromEntries(new FormData(e.target)));
    }
</script>

<h1>Day 1 - Naughty or Nice</h1>

<form on:submit|preventDefault={addChild}>
    <input type="text" name="name" placeholder="Name" required />
    <input type="number" name="tally" placeholder="Tally" required />
    <input type="submit" value="Add" />
</form>

<div class="lists">
    <div>
        <h2>Nice List 😇</h2>
        <ul class="nicelist">
            {#each nicelist as child}
                <li>{child.name} <span>({child.tally})</span></li>
            {/each}
        </ul>
    </div>
    <div>
        <h2>Naughty List 😡</h2>
        <ul class="naughtylist">
            {#each naughtylist as child}
                <li>{child.name} <span>({child.tally})</span></li>
            {/each}
        </ul>
    </div>
</div>

<style>
    form {
        padding-bottom: 1rem;
        display: flex;
        gap: 1rem;
    }
    form input {
        padding: 0.5rem;
        min-width: 5rem;
        color: initial;
    }

    form input[type="submit"] {
        min-width: fit-content;
    }

    .lists {
        display: flex;
    }
    .lists > div {
        flex: 1;
    }
    .lists ul li {
        list-style-type: none;
    }
</style>
