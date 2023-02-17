<script>
    import { goto } from "$app/navigation";
    import Button from "../../components/Button.svelte";
    import { obs } from "../../obs.js";
    import { connected } from "../../stores";

    let loggedIn = false;

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
    goto("/");
}} text="Disconnect"/>
<h1>Panel</h1>

<style>

</style>