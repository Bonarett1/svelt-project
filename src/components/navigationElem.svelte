<script lang="ts">
    let {name , selectedName = $bindable()} = $props();
    
    function clicked(){
        selectedName = name;
    }
</script>

<button class="p-2 hover:bg-green-700 active:bg-green-600 transition-all" onclick={clicked}> 
    {name}
</button>