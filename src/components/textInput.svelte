<script lang="ts">
    let text = $state("s");
    let {whatever} = $props();
    console.log()

    function click(){
        console.log(text);
        console.log(whatever)
    }
</script>
    
    <input type="text" bind:value={text} class="border-2">
    <button onclick={click}>
        
        click me!
    </button>


<style>

</style>
<div class="h-45 w-50 bg-red-500">Component testInput</div> 
