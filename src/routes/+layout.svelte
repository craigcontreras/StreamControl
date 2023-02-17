<script>
    import "../app.css";
    import { onMount } from "svelte";

    var paddingTop = "";
    var paddingBottom = "";

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
</script>

<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
</svelte:head>

<div style="padding-top: {paddingTop}; padding-bottom: {paddingBottom};">
    <slot/>
</div>