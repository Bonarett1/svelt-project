<script lang="ts">
    import ChatHistory from "$src/components/chatHistory.svelte";
    import Chatinp from "$src/components/chatinp.svelte";
    import { json } from "@sveltejs/kit";
    import axios from "axios";
    import { parse } from "cookie";

    let inputText: string = "";
    let inputElement: HTMLInputElement;
    let retVal: any;
    let inputTextPushed: string;

    function deleteChatHistory() {

        const list = document.getElementById("messages-container");

        while (list?.hasChildNodes()) {
            list.removeChild(list.firstChild!);
        }
        console.log("removed Chat history")
    }

    async function handleSendMessageStream(): Promise<void> {
        if (!inputText.trim()) return;

        // Display user message
        const userMsg = document.createElement("p");
        userMsg.className = "text-right text-blue-600";
        userMsg.textContent = inputText;
        document.getElementById("messages-container")?.appendChild(userMsg);

        inputTextPushed = inputText;
        inputText = "";

        try {
            const response = await fetch(
                "http://192.168.88.18:11434/api/chat",
                {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                        model: "qwen2.5:32b",
                        messages: [
                            {
                                role: "user",
                                content: inputTextPushed,
                            },
                        ],
                        stream: true,
                    }),
                },
            );

            if (!response.body) throw new Error("ReadableStream not supported");

            const reader = response.body.getReader();
            const decoder = new TextDecoder();
            let done = false;
            let buffer = "";

            // Create element for streaming content
            const botMsg = document.createElement("p");
            botMsg.className = "text-left text-gray-800";
            document.getElementById("messages-container")?.appendChild(botMsg);

            while (!done) {
                const { value, done: doneReading } = await reader.read();
                done = doneReading;
                if (value) {
                    buffer += decoder.decode(value, { stream: true });

                    // Process buffer by lines (assuming each chunk ends with \n)
                    let lines = buffer.split("\n");

                    // Keep incomplete line in buffer
                    buffer = lines.pop() || "";

                    for (const line of lines) {
                        if (line.trim() === "") continue;
                        try {
                            const json = JSON.parse(line);
                            // Assuming your json structure contains message.content
                            const content = json.message?.content;
                            if (content) {
                                botMsg.textContent += content;
                            }
                        } catch (e) {
                            console.warn("Failed to parse chunk as JSON", e);
                        }
                    }
                }
            }
        } catch (error) {
            console.error("Fetch error:", error);
            const errorMsg = document.createElement("p");
            errorMsg.className = "text-red-600";
            errorMsg.textContent = "Error: Could not contact server.";
            document
                .getElementById("messages-container")
                ?.appendChild(errorMsg);
        }
    }

    function handleKeyPress(event: KeyboardEvent): void {
        if (event.key === "Enter") {
            handleSendMessageStream();
        }
    }
</script>

<div
    class="bg-[url('/src/ressources/supergraphic.jpg')] bg-cover bg-center bg-no-repeat flex flex-col h-screen w-full"
>
    <div class="flex-grow flex items-center justify-center p-4">
        <div
            class="bg-white rounded-lg shadow-lg flex flex-col w-full max-w-4xl h-[80vh] overflow-hidden"
        >
            <div
                id="messages-container"
                class="flex-grow overflow-y-auto p-6 space-y-4"
            ></div>

            <div class="p-6 border-t border-gray-200 bg-white">
                <div
                    class="relative flex items-center border border-gray-300 rounded-lg shadow-sm overflow-hidden"
                >
                    <input
                        bind:value={inputText}
                        on:keypress={handleKeyPress}
                        type="text"
                        class="flex-grow p-3 text-lg focus:outline-none"
                        bind:this={inputElement}
                    />
                    <button
                        on:click={handleSendMessageStream}
                        class="p-3 text-gray-500 hover:text-gray-700 rounded-lg hover:text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
                    >
                        Send
                    </button>
                    <button
                        on:click={deleteChatHistory}
                        class="p-3 text-gray-500 hover:text-gray-700 rounded-lg hover:text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
                    >
                        Delete
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
