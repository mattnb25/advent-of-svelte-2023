<script>
    import { Map, Marker } from "@beyonk/svelte-mapbox";
    import { onMount } from "svelte";

    let mapComponent, selectedLocation, santaLocation, santaHistory;

    onMount(() => {
        // refresh page to update data, didn't bother to implement due to time
        fetch("https://advent.sveltesociety.dev/data/2023/day-twenty-four.json")
            .then((res) => res.json())
            .then((res) => {
                santaLocation = res.current || res.history[0];
                selectedLocation = santaLocation;
                santaHistory = res;
            });
    });

    function fly() {
        mapComponent.flyTo({
            center: [
                selectedLocation.location.lng,
                selectedLocation.location.lat,
            ],
            zoom: 5,
        });
    }

    function changeLocation(location) {
        selectedLocation = location;
        fly();
    }
</script>

<h1>Day 24 - Santa's Magical Tracker</h1>

<!-- wait for data to load -->
{#if santaLocation}
    <h2>Santa's current location</h2>
    <div class="current-location">
        <p>Location: {santaLocation.city}, {santaLocation.region}</p>
        <p>Arrival: {new Date(santaLocation.arrival).toTimeString()}</p>
        <p>Departure: {new Date(santaLocation.departure).toTimeString()}</p>
        <p>Population: {santaLocation.population}</p>
        <p>Presents Delivered: {santaLocation.presentsDelivered}</p>
    </div>

    <h2>Santa's trail history:</h2>
    <div class="history">
        {#each santaHistory.history as location}
            <button on:click={changeLocation(location)}>
                {location.city}, {location.region}
            </button>
        {/each}
    </div>

    <h2>Map</h2>
    <div class="map-wrapper">
        <!-- accessToken taken from https://svelte.dev/repl/f3f9da29b2fa41e2a11e258d09e51cd0?version=4.2.8 -->
        <Map
            accessToken="pk.eyJ1IjoicmljaC1oYXJyaXMiLCJhIjoiY2pzeDd4cjNmMHBtODQ0cWl5czVkMTY4ciJ9.xBTLr8gFCSZg7dqaN_B26Q"
            bind:this={mapComponent}
            on:ready={fly}
        >
            <Marker
                lat={selectedLocation.location.lat}
                lng={selectedLocation.location.lng}
                label={selectedLocation.city}
            />
        </Map>
    </div>
{:else}
    <p>loading...</p>
{/if}

<style>
    .current-location p {
        margin: 0.25rem 0;
    }

    .history {
        display: flex;
        overflow-x: auto;
        gap: 0.25rem;
    }
    .history button {
        margin-bottom: 1rem;
        white-space: nowrap;
    }

    .map-wrapper {
        height: 50dvh;
        min-height: 20rem;
        padding: 1rem 0;
    }
</style>
