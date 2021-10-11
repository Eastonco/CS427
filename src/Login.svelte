<script>
    import {user} from './user.js';

    let username;
    let password;

    function login() {
        user.auth(username, password, ({err}) => err && alert(err));
    }

    function signup() {
        user.create(username, password, ({err}) => {
            if(err) {
                alert(err);
            } else {
                login();
            }
        });
    }

</script>

<div class="loginbox">
    <label for="username">Username:</label>
    <input class="inputs" name="username" bind:value={username} minlength="3" maxlength="20" />

    <label for="password">Password:</label>
    <input class="inputs" name="password" bind:value={password} minlength="3" maxlength="20" />

    <button class="login" on:click={login}>Login</button>
    <button class="login" on:click={signup}>Sign Up</button>
</div>
