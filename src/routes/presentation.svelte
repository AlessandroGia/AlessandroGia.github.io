<script lang="ts">
    import { fade } from "svelte/transition";
    export let scroll_y: number;
    export let window_width: number;
    export let window_height: number;




</script>
    
    <div id="container" in:fade out:fade style="height: {window_height}px">
        <div class="par"></div>
        <div class="par"></div>
    </div>

<style>

    .par {
        border: 1px solid black;
    }

    #container {
        background-color: transparent;
        display: flex;
        flex-direction: column;
    }

    .div-img {
        position: absolute;
        background-color: transparent;
        width: 100%;
    }

    img {
        object-fit: fill;
        background-color: transparent;
        width: 100%;
        height: 100%;
    }

</style>