<h1>Day 10 - Pop-up! Spreading the holiday cheer</h1>

<!-- component included in +page.svelte for simplicity -->
<dialog open>
    <p>This popup wishes you a Merry Christmas. 🎅</p>
    <form method="dialog">
        <button>OK</button>
    </form>
</dialog>

<style>
    dialog {
        border: 1px solid #333;
        border-radius: 0.5rem;
        box-shadow: 0 0 1rem 0.1rem #3333;
        max-width: 95dvw;
    }
</style>
