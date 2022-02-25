<script lang="ts">
    import {createEventDispatcher} from 'svelte'
    export let person = ''
    const dispatch = createEventDispatcher()
    const handleChange = e => {
        dispatch('target-change', person)
    }
</script>

<div class="my-8 w-1/2">
    <input class="py-4 w-full px-4 rounded-xl" type="text" on:input={handleChange} bind:value={person} placeholder="Find your guy...">
</div>