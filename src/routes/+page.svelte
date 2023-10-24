<script lang="ts">
    import { io } from "socket.io-client";

    interface Message {
        author: string;
        content: string;
    }

    let message = '';

    const messages: Message[] = []
    const socket = io("sockets.hosted.costerfan5.com");

    function submit() {
        messages.push({
            author: "me",
            content: message
        })
        socket.emit("message", message);
    }

    socket.on("message", message => {
        messages.push(message)
    })
</script>

<div>
    {#each messages as message}
        <p>{message.author}: {message.content}</p>
    {/each}
</div>

<input bind:value={message} />
<button on:click={submit}>Submit</button>