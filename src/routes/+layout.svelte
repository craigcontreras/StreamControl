<script>
	import { page } from '$app/stores';
    import "./../app.css";
    import { onMount } from "svelte";
    import { panel } from "../stores.js";
    import PageTransition from "../components/PageTransition.svelte";

    let paddingTop = "";
    let paddingBottom = "";
    let onPanelPage = false;

    onMount(() => {
        document.addEventListener("deviceready", () => {
            window.plugins.safearea.get((res) => {
                paddingTop = res.top + "px";
                paddingBottom = res.bottom + "px";
            }, (err) => {
                console.log(err);
            });
        });
    });

    panel.subscribe(val => {
        onPanelPage = val;
    });
</script>

<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
</svelte:head>

<container class:bg-zinc-900={onPanelPage}>
<PageTransition refresh={$page.path}>
    <div class="px-4" style="padding-top: {paddingTop}; padding-bottom: {paddingBottom};">
        <slot/>
    </div>
</PageTransition>
</container>

<style>
    container {
        position: absolute;
        height: 100vh;
        width: 100vw;
    }
</style>