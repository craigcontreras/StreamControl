<script>
    import { goto } from "$app/navigation";
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

<button on:click={async () => {
    await obs.disconnect();
    connected.update(val => false);
    goto("/");
}}>Disconnect</button>
<h1>Panel</h1>

<style>

</style>