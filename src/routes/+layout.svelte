<script>
    import "../app.css";
    import { page } from "$app/stores";
</script>

{#if $page.url.pathname !== "/"}
    <a href="/">Home</a>
{/if}
<slot />
