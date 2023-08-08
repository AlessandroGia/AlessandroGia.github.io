<script lang="ts">

    import { fly, fade } from "svelte/transition";
    import { onDestroy, onMount } from "svelte";
    import { spring } from 'svelte/motion';

    interface circle {
        cx: number,
        cy: number
    };

    export let open: boolean = true;

    const num_circles: number = 3;
    let circles: Array<circle> = [];
    let red: number = 255 - (((new Date().getHours()) * (255/24)))
    let size = spring(0);

    let scroll_y: number;

    let window_width: number;
    let window_height: number;

    let welcome: boolean = false;

    setTimeout(() => {
        $size = window_width
        setTimeout(() => {open = false}, 1000)
    }, 2000)

    let morning = function (): boolean {
        const now: Date = new Date();
        const currentHour: number = now.getHours();
        if (currentHour >= 6 && currentHour < 18) 
            return true;
        return false;
    }


    onMount(() => {
        welcome = true;
        for(let i: number = 0; i <= num_circles; i ++) {
            circles.push({
                cx: Math.floor(Math.random() * window_width),
                cy: Math.floor(Math.random() * window_height),
            });
        }
    });

</script>


<div id="splash" out:fade={{duration:1000}}>
    {#each circles as circle}
        <svg style="opacity: 0.5; position: absolute">
            <g color="rgba({red},0,0,0.2)">
                <circle cx={circle.cx} cy={circle.cy} r={$size} fill="currentcolor"/>
            </g>
        </svg>
    {/each}

    {#if welcome}
        <span in:fade={{delay: 200}}>
            {#if morning()}
                Good Morning!
            {:else}
                Good Evening!
            {/if} 
        </span>
        <span id="p_splash" in:fly={{y: -200, duration: 1000, delay: 500}}>
            Welcome to my portfolio.
        </span>
    {/if}

</div>

<svelte:window bind:scrollY={scroll_y} bind:innerHeight={window_height} bind:innerWidth={window_width}/>

<style>

    #splash {
        width: calc(100vw - 20px);
        height: calc(100vh - 20px);
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        background-color: #ffffff;
    }

    #p_splash {
        width: 100%;
        font-size: 2em;
        text-align: center;
    }

    svg {
        width: calc(100% - 5px);
        height: calc(100% - 5px);
    }

</style>

