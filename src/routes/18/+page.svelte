<script>
    import { onMount } from "svelte";

    let questions = [];
    let status = "";

    onMount(async () => {
        fetch("https://advent.sveltesociety.dev/data/2023/day-eighteen.json")
            .then((res) => res.json())
            .then((res) => (questions = res));
    });

    let currentIndex = 0;

    function check(answer) {
        if (status) return; // already checking
        answer.correct ? (status = "Correct!") : (status = "Incorrect!");

        setTimeout(() => {
            currentIndex++;
            status = "";
        }, 1000);
    }
</script>

<h1>Day 18 - Santa's Quiz</h1>
{#each questions as question, i}
    {#if i === currentIndex}
        <p>{i + 1}: {question.question}</p>

        {#each question.answers as answer}
            <button on:click={check(answer)}>
                {answer.text}
            </button>
        {/each}
    {/if}
{/each}

<p class="status">
    {status}
</p>

{#if currentIndex === questions.length}
    <p>You've completed the quiz!</p>
{/if}

<style>
    .status {
        font-size: xx-large;
    }
</style>
