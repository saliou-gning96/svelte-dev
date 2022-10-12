<script>
    import { user } from '$lib/services/sessionStore'
    import { supabase } from '$lib/services/supabaseClient'
    import Profile from '$lib/components/Profile.svelte'
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

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

<div class="container" style="padding: 50px 0 100px 0;">
    <Profile />
</div>