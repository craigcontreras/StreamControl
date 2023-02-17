<script>
    export let text;
    export let colorLevel = 800;
</script>

<button on:click on:touchstart on:mousedown on:mouseup on:auxclick class="px-5 py-2 text-xl rounded-lg bg-zinc-{colorLevel}">{text}</button>

<style>
</style>