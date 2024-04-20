<script>
    import { onDestroy } from "svelte";

    let countdown;

    // constants for time calculations in milliseconds
    const dayInMs = 1000 * 60 * 60 * 24;
    const hourInMs = 1000 * 60 * 60;
    const minuteInMs = 1000 * 60;
    const secondInMs = 1000;

    let updateCountdown = () => {
        const midnightTime = new Date("Dec 24, 2023 23:59:59").getTime();
        const currentTime = new Date().getTime();
        const remainingTime = midnightTime - currentTime;

        const days = Math.floor(remainingTime / dayInMs);
        const hours = Math.floor((remainingTime % dayInMs) / hourInMs);
        const minutes = Math.floor((remainingTime % hourInMs) / minuteInMs);
        const seconds = Math.floor((remainingTime % minuteInMs) / secondInMs);

        countdown = `${days}d ${hours}h ${minutes}m ${seconds}s`;
    };

    // start instantly
    updateCountdown();

    // update the countdown every second
    const interval = setInterval(updateCountdown, 1000);

    onDestroy(() => {
        clearInterval(interval);
    });
</script>

<h1>Day 9 - Santa's Final Countdown</h1>
<p>Christmas 2023 was in:</p>
{#if countdown}
    <p class="timer">{countdown}</p>
{/if}

<style>
    .timer {
        font-size: xx-large;
    }
</style>
