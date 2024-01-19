<script>
    let expenses = [];

    let limit = 0;
    $: spent = expenses.reduce((total, expense) => {
        return total + expense.cost;
    }, 0);

    function addExpense(e) {
        // map the FormData into an object then add it to the array
        expenses = [...expenses, Object.fromEntries(new FormData(e.target))];
    }
</script>

<h1>Day 17 - Festive Funds</h1>
<p>Manage your budget with this simple tool.</p>

<h2>Budget</h2>
<label for="budget">Budget limit: </label>
<input type="number" id="budget" bind:value={limit} />
<p>Total budget remaining: ${(limit - spent).toFixed(2)}</p>

<h2>Expenses</h2>
<form on:submit|preventDefault={addExpense}>
    <label for="name">Name</label>
    <input type="text" name="name" id="name" />

    <label for="cost">Cost</label>
    <input type="number" name="cost" id="cost" />

    <input type="submit" value="Track expense" />
</form>

{#each expenses as expense}
    <p>{expense.name}: ${expense.cost}</p>
{/each}

<style>
    input {
        display: block;
        margin: 0.25rem 0;
    }
    input[type="submit"] {
        margin: 1rem 0;
    }
</style>
