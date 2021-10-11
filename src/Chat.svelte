<script>
    import Login from './Login.svelte';
    import ChatMessage from './ChatMessage.svelte';
    import { username, user} from './user';
    import { onMount } from 'svelte';

    import GUN from 'gun';
    const db = GUN();

    let newMessage;
    let messages = [];

    onMount(() => {
        db.get('chat').map().once(async (data, id) => {
            if (data) {
                const key = '#foo';

                var message = {
                    who: await db.user(data).get('alias'),
                    what: data.what,
                    when: GUN.state.is(data, 'what'),

                }

                if(message.what) {
                    messages = [...messages.slice(-100), message];
                }
            }
        });
    })

    async function sendMessage() {
        const message = user.get('all').set({what:newMessage});
        const index = new Date().toISOString();
        db.get('chat').get(index).put(message);
        newMessage = '';
    }
</script>

<div class="container">
    {#if $username}
        <main>
            {#each messages as message (message.when)}
                <ChatMessage message={message} sender={$username}/>
            {/each}
        </main>

        <form on:submit|preventDefault={sendMessage}>
            <input type="text" placeholder="Type a message..." bind:value={newMessage}/>
            <button type="submit" disabled={!newMessage}>Send</button>
        </form>
    {:else}
        <main>
            <Login />
        </main>
    {/if}
</div>