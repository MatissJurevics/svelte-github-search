<script lang="ts">
    import UserCard from "./userCard.svelte";
    export let userList;
</script>

<div class="grid grid-cols-3 w-1/2">
    {#each userList as user (user.id)}
    <UserCard on:detailed-view id={user.id} login={user.login} avatar={user.avatar_url} pageLink={user.html_url}/>
    {/each}
</div>