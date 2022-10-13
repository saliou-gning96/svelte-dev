<script>
    import { user } from '$lib/services/sessionStore'
    import { supabase } from '$lib/services/supabaseClient'
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import Header from '$lib/components/Header.svelte';

    user.set(supabase.auth.user());

    supabase.auth.onAuthStateChange((_, session) => {
        user.set(session.user)
    })

    onMount(() => {
        if (null === $user) {
            goto('/login')
        }
    })
</script>

<div class="container">
    <Header />
      
    <slot></slot>
</div>