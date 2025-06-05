<script lang="ts">
    import ChatHistory from "$src/components/chatHistory.svelte";
    import Chatinp from "$src/components/chatinp.svelte";

</script>


<div class="border-2 bg-red-50">


</div>
<slot>

</slot>
<div class="w-4/5">
    <ChatHistory input={"erzähl mir was über die kantine im 2ten weltkrieg aus der sicht eines amerikanischen soldate"}> </ChatHistory>
</div>
<Chatinp> </Chatinp>