<script>
    import { username, user} from './user'

    function signout() {
        user.leave()
        username.set('')
    }
</script>


<header>
    <h1>Chit Chat</h1>
    {#if $username}
        <div class="user-bio">
            <span>Hello <strong>{$username}</strong></span>
            <img src={`https://avatars.dicebear.com/api/identicon/${$username}.svg`} alt="avatar"/>
        </div>

        <button class="signout-button" on:click={signout}>Sign out</button>
    
    {:else}
        <h3>Powered by Gun.js & Svelte</h3>
    {/if}
</header>