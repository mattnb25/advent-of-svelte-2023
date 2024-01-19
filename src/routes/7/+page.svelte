<script>
    import { onDestroy } from "svelte";

    // user input, Morse code output, global audio context, and Morse code dictionary
    let input;
    let output;
    let audioCtx;
    let morse = {
        A: ".-",
        B: "-...",
        C: "-.-.",
        D: "-..",
        E: ".",
        F: "..-.",
        G: "--.",
        H: "....",
        I: "..",
        J: ".---",
        K: "-.-",
        L: ".-..",
        M: "--",
        N: "-.",
        O: "---",
        P: ".--.",
        Q: "--.-",
        R: ".-.",
        S: "...",
        T: "-",
        U: "..-",
        V: "...-",
        W: ".--",
        X: "-..-",
        Y: "-.--",
        Z: "--..",
        "0": "-----",
        "1": ".----",
        "2": "..---",
        "3": "...--",
        "4": "....-",
        "5": ".....",
        "6": "-....",
        "7": "--...",
        "8": "---..",
        "9": "----.",
        " ": "/",
    };

    // convert input to Morse code whenever it changes
    $: if (input) {
        output = input
            .toUpperCase()
            .split("")
            .map((char) => morse[char] || "")
            .join(" ");
    } else {
        output = "Waiting for input";
    }

    // play audio based on the Morse code output
    function play() {
        if (audioCtx) audioCtx.close(); // close any existing audio context
        audioCtx = new AudioContext(); // create a new audio context

        output.split("").forEach((note, i) => {
            // create an oscillator for each character
            const o = audioCtx.createOscillator();
            o.connect(audioCtx.destination);

            const speed = 5; // optionally increases the speed of playback
            const start = i / speed; // playback time is based on its index

            if (note === ".") {
                o.start(start);
                o.stop(start + 0.25 / speed);
            } else if (note === "-") {
                o.start(start);
                o.stop(start + 0.75 / speed);
            }
        });
    }

    // stop audio when the component is destroyed
    onDestroy(() => {
        if (audioCtx) audioCtx.close();
    });
</script>

<h1>Day 7 - Morse Mischief</h1>

<textarea
    bind:value={input}
    maxlength="1000"
    placeholder="Type here to convert your text into Morse code"
></textarea>

<h2>Morse</h2>
<p>{output}</p>
<button on:click={play}>Play</button>

<style>
    textarea {
        width: 100%;
    }
    p {
        font-family: monospace;
    }
</style>
