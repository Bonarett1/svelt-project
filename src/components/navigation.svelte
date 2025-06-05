<script lang="ts">
    import { onMount } from "svelte";
    import NaviElem from "./navigationElem.svelte"
    import axios from "axios";
    import { json } from "@sveltejs/kit";
    import NavigationElem from "./navigationElem.svelte";
    
    let selected:string = $props();
    let models:any = $state([1,2]);
    const requestModels = async () => {
        const request =await axios.get("http://192.168.88.18:11434/api/tags")
        console.log( JSON.stringify(request.data))
        models = request.data.models;

    }

    

    onMount(async ()=>{
        
        await requestModels();

    });
    
</script>


<div class="personality__selector__wrapper hover:bg-gray-200 transition-all">
    <h2 class="align-middle w-full text-center">
        Personality Selector
    </h2>

    <div class="personality__buttons__wrapper flex flex-row justify-center">
    
        {#each models as model }

       <NavigationElem name = {model.name}/>
        {/each}

    
    </div>

</div>




