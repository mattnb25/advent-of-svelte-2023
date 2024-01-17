<script>
    import { onMount } from "svelte";

    // jokes array, current joke, and the speech synthesis object
    let jokes, joke, synth;

    onMount(() => {
        fetch("https://advent.sveltesociety.dev/data/2023/day-twenty.json")
            .then((res) => res.json())
            .then((res) => (jokes = res));

        // reference to window.speechSynthesis requires onMount
        synth = window.speechSynthesis;
        return () => synth.cancel(); // stop speech on page exit
    });

    function playJoke() {
        synth.cancel(); // stop any previous speech
        joke = jokes[Math.floor(Math.random() * jokes.length)] || "error";
        speechSynthesis.speak(new SpeechSynthesisUtterance(joke));
    }
</script>

<h1>Day 20 - Jingle Jokes</h1>

<button on:click={playJoke}>Play a joke</button>
<p>{joke || ""}</p>
