<script>
    import { supabase } from '$lib/services/supabaseClient'
    import { onMount } from 'svelte';

    let loading = true
    let users = []

    onMount(() => {
        getUsers();
    })

    function getUsers() {
        try {
            loading = true
            supabase
                .from('profiles')
                .select(`username, website, avatar_url`)
                .then(({ data, error, status }) => {
                    if (data) {
                        users = data
                    }
                    if (error && status !== 406) throw error
                }
            )
        } catch (error) {
            alert(error.message)
        } finally {
            loading = false
        }
    }
</script>

<table class="table table-striped table-bordered">
    <thead>
        <tr>
            <th>Name</th>
            <th>Website</th>
        </tr>
    </thead>
    <tbody>
        {#each users as user (user.id)}
        <tr>
            <td>{user.username}</td>
            <td>{user.website}</td>
        </tr>
        {/each}
    </tbody>
</table>
