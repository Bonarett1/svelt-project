<script lang="ts">
<<<<<<< Updated upstream
  import ChatHistory from "$src/components/chatHistory.svelte";
  import Chatinp from "$src/components/chatinp.svelte";
  import { json } from "@sveltejs/kit";
  import axios from "axios";
  import { parse } from "cookie";

  let inputText: string = "";
=======
	import ChatHistory from "$src/components/chatHistory.svelte";
	import Chatinp from "$src/components/chatinp.svelte";
  import Navigation from "$src/components/navigation.svelte";
  import axios from "axios";
   import { json } from "@sveltejs/kit"
	
  let inputText: string = $state("");
>>>>>>> Stashed changes
  let inputElement: HTMLInputElement;
  let retVal: any;
  let inputTextPushed: string;

 
  let selectedPersonality = $state("");
  let retVal: any = null;
  let fetchError: any = null; 

  const requestAi = async (input:string,aimodel:string) => {
    try {
        const res = await axios.post("http://192.168.88.18:11434/api/chat", {
            model: aimodel,    
            messages: [
                
                {
                    role: "user",
                    content: input
                }
            ],
            stream: false
        });
        retVal = res.data;
        return json.parse(retVal);
    } catch (e) {
        fetchError = e;
        console.error("Fetch error:", e);
    }
  }
  async function handleSendMessage(): Promise<void> {
<<<<<<< Updated upstream
    const child = document.createElement("p");
    child.textContent = inputText;
    document.getElementById("messages-container")?.appendChild(child);
    inputTextPushed = inputText;
    console.log(inputText);
    try {
      const res = await axios.post("http://192.168.88.18:11434/api/chat", {
        model: "qwen2.5:32b",
        messages: [
          {
            role: "user",
            content: inputText,
          },

        ],
        stream: true,
      });
      retVal = res.data;
      const content = retVal?.message?.content ?? "No content returned";

      // Append bot response
      const botMsg: HTMLParagraphElement = document.createElement("p");
      botMsg.className = "text-left text-gray-800";
      botMsg.textContent = content;
      document.getElementById("messages-container")?.appendChild(botMsg);
    } catch (e) {
      console.error("Fetch error:", e);
    }
=======
    let response = await requestAi(inputText);
    console.log(response);
>>>>>>> Stashed changes
  }

  function handleKeyPress(event: KeyboardEvent): void {
    if (event.key === "Enter") {
      handleSendMessage();
    }
  }
  
</script>

<div
  class="bg-[url('/src/ressources/supergraphic.jpg')] bg-cover bg-center bg-no-repeat flex flex-col h-screen w-full"
>
  <div class="flex-grow flex items-center justify-center p-4">
<<<<<<< Updated upstream
    <div
      class="bg-white rounded-lg shadow-lg flex flex-col w-full max-w-4xl h-[80vh] overflow-hidden"
    >
      <div
        id="messages-container"
        class="flex-grow overflow-y-auto p-6 space-y-4"
      ></div>
=======
    <div class="bg-white rounded-lg shadow-lg flex flex-col w-full max-w-4xl h-[80vh] overflow-hidden">
      <Navigation/>
      <div id="messages-container" class="flex-grow overflow-y-auto p-6 space-y-4">

      </div>
>>>>>>> Stashed changes

      <div class="p-6 border-t border-gray-200 bg-white">
        <div
          class="relative flex items-center border border-gray-300 rounded-lg shadow-sm overflow-hidden"
        >
          <input
            bind:value={inputText}
            onkeypress={handleKeyPress}
            type="text"
            class="flex-grow p-3 text-lg focus:outline-none"
            bind:this={inputElement}
          />
          <button
<<<<<<< Updated upstream
            on:click={handleSendMessage}
            class="p-3 text-gray-500 hover:text-gray-700 rounded-lg hover:text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
=======
            onclick={handleSendMessage}
            class="p-3 text-gray-500 hover:text-gray-700 hover:text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
>>>>>>> Stashed changes
          >
            Send
          </button>
        </div>
      </div>
    </div>
  </div>
</div>
