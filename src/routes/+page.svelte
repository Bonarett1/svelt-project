<script lang="ts">
    import ChatHistory from "$src/components/chatHistory.svelte";
    import Chatinp from "$src/components/chatinp.svelte";

</script>

<h1>Welcome to your library project</h1>
<p>Create your package using @sveltejs/package and preview/showcase your work with SvelteKit</p>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>

<div class="border-2 bg-red-50">


</div>
<slot>

</slot>
<Chatinp></Chatinp>
<ChatHistory input={"hallo borchewrs"}> </ChatHistory>