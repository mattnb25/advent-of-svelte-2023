<script>
    import { onDestroy } from "svelte";

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

    $: if (input) {
        output = input
            .toUpperCase()
            .split("")
            .map((char) => morse[char] || "")
            .join(" ");
    } else {
        output = "Waiting for input";
    }

    function play() {
        if (audioCtx) audioCtx.close();
        audioCtx = new AudioContext();

        output.split("").forEach((note, i) => {
            const o = audioCtx.createOscillator();
            o.connect(audioCtx.destination);

            const speed = 5;
            const start = i / speed;

            if (note === ".") {
                o.start(start);
                o.stop(start + 0.25 / speed);
            } else if (note === "-") {
                o.start(start);
                o.stop(start + 0.75 / speed);
            }
        });
    }

    onDestroy(() => {
        if (audioCtx) audioCtx.close();
    });
</script>

<h1>Day 7 - Morse Mischief</h1>

<textarea bind:value={input} maxlength="1000" placeholder="Type here"
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
