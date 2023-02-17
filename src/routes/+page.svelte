<script>
	import { connected } from './../stores.js';
    import { obs } from "./../obs.js";
    import { goto } from '$app/navigation';
    import Button from '../components/Button.svelte';
    import { gsap } from "gsap";

    let ip = "";
    let port = 0;
    let password = "";
    let loggedIn = false;
    let buttonAnimationElement;

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
    <div class="text-center mt-[25%]">
        <Button colorLevel=900 on:click={async () => {
            try {
                const connection = await obs.connect(`ws://${ip}:${port}`, password, {});
                connected.update(val => true);
                const timeline = gsap.timeline();

                timeline.to(buttonAnimationElement, {
                    scaleX: 500,
                    duration: 0.5,
                    opacity: 100,
                    transformOrigin: "center",
                    ease: "power1.in",
                }).to(buttonAnimationElement, {
                    scaleY: 500,
                    duration: 0.5,
                    ease: "power2.in"
                }, "-=0.1");
                setTimeout(() => {
                    goto("/panel");
                }, 1000);
            } catch (err) {
                console.log(err);
                connected.update(val => false);
            }
        }} classes="relative" text="Submit"/>    
        <div bind:this={buttonAnimationElement} class="fixed left-[50%] opacity-0 bg-zinc-900 h-1 w-2 -z-10"></div>
    </div>
</form>

<p>{loggedIn}</p>

<style>

</style>