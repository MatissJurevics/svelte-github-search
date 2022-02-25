<script lang="ts">
    import {createEventDispatcher} from 'svelte'
    export let id:number;
    export let login:string;
    export let avatar:string;
    export let pageLink:string;
    const dispatch = createEventDispatcher()
    let user = {
            id:id,
            login:login,
            avatar:avatar,
            pageLink:pageLink
        }
    const handleSelect = () => {
        dispatch('detailed-view', user)
        
    }
</script>

<div on:click={handleSelect} class=" cursor-pointer relative m-4 border bg-white drop-shadow-lg border-slate-200 rounded-xl p-4">
    <img class="w-16 h-16 rounded-full" src="{avatar}" alt="avatar">
    <a class="absolute top-3 right-1/2 translate-x-1/2 text-blue-600 font-medium" href="{pageLink}">{login}</a>
    <h2 class="absolute top-3 right-3">{id}</h2>
</div>