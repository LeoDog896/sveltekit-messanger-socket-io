<script lang="ts">
    import { io } from "socket.io-client";

    type Message = {
        author: string,
        content: string,
        server?: {
            online: number,
            userList: String[]
        }
    }


    let message = '';

    let messages: Message[] = []
    const socket = io("https://sockets.hosted.coasterfan5.com/");

    function submit() {
        socket.emit("setName", "Goover");
        messages = [...messages, {
            author: "me",
            content: message
        }]
        socket.emit("message", message);
    }

    socket.on("message", message => {
        messages = [...messages, message]
    })
</script>

<div>
    {#each messages as message}
        <p>{message.author}: {message.content}</p>
    {/each}
</div>

<input bind:value={message} />
<button on:click={submit}>Submit</button>
