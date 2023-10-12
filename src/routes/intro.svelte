<script lang="ts">
    import { fade, fly } from "svelte/transition";

    export let scroll_y: number;


    const birthday: string = '2001-09-24';

    export let window_width: number;
    export let window_height: number;
    

</script>

<div id="intro" in:fly={{y: 100, duration: 500}} style="width:{window_width}px; height:{window_height}px; background-color: transparent">
    
    {#if scroll_y < window_height/2}
        <div id="segment_1" in:fade={{delay:500}} out:fade></div>
        <div id="segment_2" in:fly={{y: 200, duration: 1000}} out:fly={{y: -200, duration: 1000}} style="background-color: ; width: {window_width}px; height: {window_height}px; left: {window_width * 0.2}px; top: -{window_height/2}px; transform: rotate({40 + scroll_y/5}deg);"></div>
        <div id="segment_3" in:fly={{y: -200, duration: 1000}} out:fly={{y: 200, duration: 1000}} style="background-color: ; width: {window_width}px; height: {window_height}px; right: {window_width * 0.2}px; bottom: -{window_height/2}px; transform: rotate({60 + scroll_y/5}deg);"></div>

        <div id="about_me" in:fly={{x: 500, duration: 1000, delay:500}} out:fade={{delay:500}}>
            <span in:fade={{duration: 1000, delay: 1000}}>
                I'm Alessandro Giacento, a {new Date().getFullYear() - new Date(birthday).getFullYear()}-years-old Computer Science student at The University of Pesche.
            </span>
        </div>
    {/if}
    
</div>




<style>

    #intro {
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        background-color: transparent;
        overflow: hidden;
    }

    #segment_1 {
        position: absolute;
        background-color: green;
        width: 100%;
        height: 100%;
    }

    #segment_2 {
        position: absolute;
        background-color: red;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }

    #segment_3 {
        position: absolute;
        background-color: white;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }

    #about_me {
        position: absolute;
        align-items: center;
        font-size: 1.5em;
        padding: 5vw;
        margin-right: 5vw;
        margin-left: 5vw;
        background-color: #cccccc;
        border-radius: 25px;
        opacity: 0.8;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }
    
</style>