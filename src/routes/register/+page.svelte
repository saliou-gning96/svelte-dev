<script>
    import { supabase } from '$lib/services/supabaseClient'
    import { goto } from '$app/navigation';
  
    let loading = false
    let email
    let password = ''
    let confirmPassword = ''
  
    const handleLogin = async () => {
      try {
        loading = true
        const { error } = await supabase.auth.signUp({ email: email, password: password })
        if (error) throw error
        goto('/')
      } catch (error) {
        alert(error.error_description || error.message)
      } finally {
        loading = false
      }
    }

    $: isPasswordValid = password !== confirmPassword;

</script>
  
<form class="row flex-center flex" on:submit|preventDefault="{handleLogin}">
    <div class="col-6 form-widget">
        <h1 class="header">Création de compte</h1>
        <p class="description">Créeation simple de compte</p>
        <div>
            <input
                class="inputField"
                type="email"
                placeholder="Your email"
                bind:value="{email}"
            />
        </div>
        <div>
            <input
                class="inputField"
                type="password"
                placeholder="Your password"
                bind:value="{password}"
            />
        </div>
        <div>
          <input
              class="inputField"
              type="password"
              placeholder="Confirm your password"
              bind:value="{confirmPassword}"
          />
      </div>
        <div>
        <input type="submit" class='button block' value={loading ? "Loading" :
        "REGISTER"} disabled={loading || isPasswordValid} />
        </div>
    </div>
</form>