<script lang="ts">
    import { json } from "@sveltejs/kit";
    import axios from "axios";
    import { onMount } from "svelte";

    export let input :string

    let retVal: any = null;
    let fetchError: any = null; 

    onMount(async () => {
        try {
            const res = await axios.post("http://192.168.88.18:11434/api/chat", {
                model: "xorchers:latest",    
                messages: [
                    
                    {
                        role: "user",
                        content: input
                    }
                ],
                stream: false
            });
            retVal = res.data;
            json.parse(retVal)
            console.log(retVal);
        } catch (e) {
            fetchError = e;
            console.error("Fetch error:", e);
        }
    });
</script>

<div>{input}</div>
<div class="wrap-anywhere w-4/5">
{#if retVal}
    <p>{JSON.stringify(retVal.message.content, null, 2)}</p>
{:else if fetchError}
    <p>Error: {fetchError.message}</p>
{:else}
    <p>Loading...</p>
{/if}
</div>