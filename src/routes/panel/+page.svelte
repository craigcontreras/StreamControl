<script>
    import { goto } from "$app/navigation";
    import { onMount, onDestroy } from "svelte";
    import Button from "../../components/Button.svelte";
    import { obs } from "../../obs.js";
    import { connected, panel } from "../../stores.js";

    let loggedIn = false;

    onMount(() => {
        panel.update(val => true);
    });

    onDestroy(() => {
        panel.update(val => false);
    });

    window.onhashchange = () => {
        panel.update(val => false);
    };

    connected.subscribe(val => {
        loggedIn = val;
    });

    if (!loggedIn) {
        goto("/");
    }

    obs.on("close", () => {
        connected.update(val => false);
    });
</script>

<Button on:click={async () => {
    await obs.disconnect();
    connected.update(val => false);
    panel.update(val => false);
    goto("/");
}} text="Disconnect"/>
<h1>Panel</h1>

<style>

</style>