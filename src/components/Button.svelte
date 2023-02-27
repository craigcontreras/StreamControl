<script>
    import { gsap } from "gsap";
    import { onMount } from "svelte";
    
    export let text;
    export let colorLevel = 800;
    let button;
    let animation;

    onMount(() => {
        animation = gsap.to(button, {
            scale: 0.95,
            yoyo: true,
            paused: true,
            repeat: 1,
            duration: 0.05,
        });
    })
</script>

<button bind:this={button} on:click={() => {
    animation.restart();
}} on:click on:touchstart on:mousedown on:mouseup on:auxclick class="px-5 py-2 text-xl rounded-lg bg-zinc-{colorLevel}">{text}</button>

<style>
</style>