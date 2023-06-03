<script>
    import { initializeApp } from 'firebase/app'
    import { getAuth, signInWithEmailAndPassword } from 'firebase/auth'
    import jwtDecode from 'jwt-decode'
    
    const firebaseConfig = {
        // Your firebase config here
    }
    const app = initializeApp(firebaseConfig)
    const auth = getAuth(app)

    let email = ''
    let password = ''
    let user = null
    let token = null

    async function login() {
        try {
            user = (await signInWithEmailAndPassword(auth, email, password)).user
            token = jwtDecode(user.accessToken)
            console.log(user)
        } catch(e) {
            alert('Error logging in:', e)
        } finally {
            email = ''
            password = ''
        }
    }
</script>

<main>
    <form>
        <label for="email">Email</label>
        <input id="email" type="email" bind:value={email} />

        <label for="password">Password</label>
        <input id="password" type="password" bind:value={password} />

        <button on:click={login}>Login</button>
    </form>
</main>

<aside>
    {#if user}
        <div><b>accessToken:</b></div>
        <div>{user?.accessToken}</div>
        <hr/>
        <div><b>email:</b> {user?.email}</div>
        <div><b>firebase id:</b> {token.sub}</div>
        <div><b>roles:</b> </div>
    {:else}
        No Logged-in User
    {/if}
    
</aside>

<style>
    form {
        width: 80%;
        display: flex;
        flex-direction: column;
    }

    input {
        margin-bottom: 1rem;
    }

    aside > div {
        margin-bottom: 1rem;
    }
    
</style>
