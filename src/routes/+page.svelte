<script>
	import { connected } from './../stores.js';
    import { obs } from "./../obs.js";
    import { goto } from '$app/navigation';

    let ip = "";
    let port = 0;
    let password = "";
    let loggedIn = false;

    connected.subscribe(val => {
        loggedIn = val;
    });

    if (loggedIn) {
        goto("/panel");
    }
</script>

<h1>Log in</h1>
<form action="" on:submit|preventDefault>
    <input type="text" bind:value={ip}>
    <input type="number" bind:value={port}>
    <input type="password" bind:value={password}>
    <button on:click={async () => {
        try {
            const connection = await obs.connect(`ws://${ip}:${port}`, password, {});
            connected.update(val => true);
            goto("/panel");
        } catch (err) {
            connected.update(val => false);
        }
    }}>Submit</button>    
</form>

<p>{loggedIn}</p>

<style>

</style>