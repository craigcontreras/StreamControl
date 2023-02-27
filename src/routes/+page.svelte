<script>
	import { Main } from 'capacitor-plugin-native-compass';
	import { connected } from './../stores.js';
    import { obs } from "./../obs.js";
    import { goto } from '$app/navigation';
    import Button from '../components/Button.svelte';
    import { gsap } from "gsap";
    import { onMount } from 'svelte';

    let ip = ""; 
    let port = 0;
    let password = "";
    let loggedIn = false;
    let buttonAnimationElement;
    
    let ipBackingElement;
    let ipBackingElementAnimation;
    
    let portBackingElement;
    let portBackingElementAnimation;
    
    let passBackingElement;
    let passBackingElementAnimation;

    let ipHeader;
    let eventTest;

    connected.subscribe(val => {
        loggedIn = val;
    });

    if (loggedIn) {
        goto("/panel");
    }

    onMount(async () => {
        document.addEventListener("deviceready", async () => {
            eventTest = "neither";
            setInterval(() => {
                Main.startMonitoring().then((evt) => {
                    eventTest = JSON.stringify(evt);
                }).catch((err) => {
                    eventTest = err;
                });
            }, 10);
            navigator.compass.watchHeading((event) => {
                var y = event.magneticHeading * (3.14/180);
                var clamped = clamp(y, -1, 1);
                ipHeader.style.transform = "rotateY(" + clamped + "rad)";
            }, () => {

            }, {
                frequency: 10,
            });

            
            // let fromX = 0;
            // let fromY = 0;
            // navigator.gyroscope.watchGyroscope((event) => {
            //     eventTest = event;
            //     ipHeader.style.transform = "rotateX(" + x + "rad) rotateY(" + y + "rad)";
            // }, (err) => {
            //     eventTest = err;
            // }, {
            //     frequency: 50,
            // })
        });

        ipBackingElementAnimation = gsap.to(ipBackingElement, {
            scale: 1.2,
            yoyo: true,
            paused: true,
            repeat: -1,
        });
        portBackingElementAnimation = gsap.to(portBackingElement, {
            scale: 1.2,
            yoyo: true,
            paused: true,
            repeat: -1,
        });
        passBackingElementAnimation = gsap.to(passBackingElement, {
            scale: 1.2,
            yoyo: true,
            paused: true,
            repeat: -1,
        });
    });
    const clamp = (num, min, max) => Math.min(Math.max(num, min), max);
</script>
<!-- <button on:click={async () => {
    try {
        await DeviceMotionEvent.requestPermission();
        await DeviceOrientationEvent.requestPermission();
    } catch (err) {
        return;
    }

    accelHandler = await Motion.addListener("orientation", event => {
        let x = event.beta;
        let y = event.gamma;
        let z = event.alpha;
        ipHeader.style.transform = "rotateX(" + x + "deg) rotateY(" + y + "deg) rotateZ(" + z + "deg)";
    });
}}>Test</button> -->

{eventTest}
<container class="fixed top-[50%] left-[50%] -translate-y-[50%] -translate-x-[50%]">
    <form action="" on:submit|preventDefault>
        <div class="ip" bind:this={ipHeader}>
            <h2 class="header inline-block origin-center text-6xl font-black relative top-3 -z-10">IP</h2>
            <input on:mouseenter={() => {
                ipBackingElementAnimation.restart();
            }} on:mouseleave={() => {
                ipBackingElementAnimation.pause();
                gsap.to(ipBackingElement, {
                    scale: 1,
                });
            }} class="relative p-4 rounded-lg hover:drop-shadow-xl hover:-translate-y-2 outline-none transition-all" type="text" bind:value={ip}>
            <input disabled bind:this={ipBackingElement} class="absolute left-3 -z-10 py-4 px-1 rounded-lg bg-black opacity-10" type="text">
        </div>
        <div class="port">
            <h2 class="header text-6xl font-black relative top-3 -z-10">Port</h2>
            <input on:mouseenter={() => {
                portBackingElementAnimation.restart();
            }} on:mouseleave={() => {
                portBackingElementAnimation.pause();
                gsap.to(portBackingElement, {
                    scale: 1,
                });
            }} class="relative p-4 rounded-lg hover:drop-shadow-xl hover:-translate-y-2 outline-none transition-all" type="number" bind:value={port}>
            <input disabled bind:this={portBackingElement} class="absolute left-3 -z-10 py-4 px-1 rounded-lg bg-black opacity-10" type="text">
        </div>
        <div class="password">
            <h2 class="header text-6xl font-black relative top-3 -z-10">Password</h2>
            <input on:mouseenter={() => {
                passBackingElementAnimation.restart();
            }} on:mouseleave={() => {
                passBackingElementAnimation.pause();
                gsap.to(passBackingElement, {
                    scale: 1,
                });
            }} class="p-4 rounded-lg hover:drop-shadow-xl hover:-translate-y-2 outline-none transition-all" type="password" bind:value={password}>
            <input disabled bind:this={passBackingElement} class="absolute left-3 -z-10 py-4 px-1 rounded-lg bg-black opacity-10" type="text">
        </div>
        <div class="text-center mt-[5%]">
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
                        zIndex: 10,
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
            }} classes="relative" text="Login"/>    
            <div bind:this={buttonAnimationElement} class="fixed left-[50%] opacity-0 bg-zinc-900 h-1 w-2 -z-10"></div>
        </div>
    </form>
</container>

<style>
    .header {
        background: -webkit-linear-gradient(#eee, #333);
        -webkit-background-clip: text;
        background-clip: text;
        -webkit-text-fill-color: transparent;
    }
</style>