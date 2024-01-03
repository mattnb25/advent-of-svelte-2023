<script>
    let firstName, lastName, elfName;

    function getName() {
        fetch("https://advent.sveltesociety.dev/data/2023/day-eleven.json")
            .then((x) => x.json())
            .then((elfNames) => {
                let firstNames = elfNames.firstNames;
                let lastNames = elfNames.lastNames;

                // filter names by initials
                firstNames = firstNames.filter(
                    (name) =>
                        name[0].toUpperCase() === firstName[0].toUpperCase(),
                );
                lastNames = lastNames.filter(
                    (name) =>
                        name[0].toUpperCase() === lastName[0].toUpperCase(),
                );

                // select name by random index
                const elfFirstName =
                    firstNames[Math.floor(Math.random() * firstNames.length)] ||
                    "Twinkle";
                const elfLastName =
                    lastNames[Math.floor(Math.random() * lastNames.length)] ||
                    "McJingles";

                elfName = elfFirstName + " " + elfLastName;
            })
            .catch((elfName = "(an error occured)"));
    }
</script>

<h1>Day 11 - Tinsel Transformers</h1>

<form action="">
    <p>Enter your name below to convert it into an elf name.</p>
    <input type="text" placeholder="First" bind:value={firstName} required />
    <input type="text" placeholder="Last" bind:value={lastName} required />
    <button type="submit" on:click={getName}>Go</button>
</form>

<p>Your elf name is: {elfName || "waiting..."}</p>

<style>
    input {
        margin-bottom: 0.25rem;
    }
</style>
