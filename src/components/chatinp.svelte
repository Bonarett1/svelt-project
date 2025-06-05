<script lang="ts">
    import type { svelte } from "@sveltejs/vite-plugin-svelte";
    import {createEventDispatcher} from "svelte";

    let input = $state("")

    function senddata(){
        return
    }

</script>

<div class="h-10 w-4/5 bg-gray-500" > <input value={input} type=" text" class="h-full w-full"></div> 
<button onclick={senddata}></button>