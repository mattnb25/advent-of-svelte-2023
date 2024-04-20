<script>
    import { onMount } from "svelte";

    let latitude, longitude, weather;

    onMount(async () => {
        navigator.geolocation.getCurrentPosition((position) => {
            latitude = position.coords.latitude;
            longitude = position.coords.longitude;

            // nested fetch ensures that values are available before fetch
            fetch(
                `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current=temperature_2m,snowfall,wind_speed_10m,wind_direction_10m`,
            )
                .then((res) => res.json())
                .then((res) => (weather = res.current)); // store a portion of the data
            console.log("sdf");
        });
    });
</script>

<h1>Day 21 - Oh the weather outside is frightful</h1>

{#if !weather}
    <p>Attempting to load. Please enable location access.</p>
{:else}
    <h2>
        Weather at {latitude.toFixed(5)}
        {longitude.toFixed(5)}
    </h2>
    <p>🌡️ Temperature: {weather.temperature_2m}°C</p>
    <p>❄️ Snowfall: {weather.snowfall}cm</p>
    <p>🍃 Wind Speed: {weather.wind_speed_10m}km/h</p>
    <p>➡️ Wind Direction: {weather.wind_direction_10m}°</p>
{/if}
