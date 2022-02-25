<script lang="ts">
    import {createEventDispatcher} from 'svelte'
    import { fade } from 'svelte/transition'
    import axios from 'axios'

    export let on:boolean;
    export let userData:string;


    const dispatch = createEventDispatcher()
    const handleExit = () => {
        dispatch('exit-selected')
    }
    const getData = async () => {
        try{
            const res = await axios(`https://api.github.com/users/${userData.login}/repos`)
            return res.data
        } catch (err) {
            console.log(err)
        }
    }
    let user = null
    if (on) {
        user = getData()
        console.log(user)
    }
</script>

<main class="fixed  w-screen h-screen flex flex-col justify-center items-center ">
    <backg transition:fade on:click={handleExit} class="fixed w-screen h-screen  bg-black bg-opacity-40">
    
    </backg>

    <div transition:fade class="relative h-3/4 w-3/4 z-10 bg-white opacity-100 rounded-2xl p-8">
        {#if on}
        {#await user}
        <h1>loading </h1>
        {:then user}
        <div class="flex flex-row"> 
            <img class="w-32 h-32 rounded-full border-blue-600 border drop-shadow-xl" src="{userData.avatar}" alt="Profile Picture">
            <div>
                <h1 class="text-3xl font-semibold pl-4">{userData.login}</h1>  
                <h1 class="text-lg font-light pl-4 text-slate-600">ID: {userData.id}</h1>
            </div>
        </div>
        <div class="pt-4 w-full h-3/4 ">
            <h1 class="text-4xl py-2 font-bold w-full border-b border-slate-300">Repositories</h1>
            <div class="overflow-auto w-full max-h-full">
                {#each user as us (us.id)}
                <li class="border my-4 mr-2 py-2 px-2 rounded-md flex justify-between items-center" >
                    <div>
                        <a class="text-xl text-blue-700 font-semibold" href="{us.html_url}">{us.name}</a>
                        <p class="font-light text-slate-600">Clone url: "{us.clone_url}"</p>
                    </div>
                    <a class="mr-4 py-2 px-4 rounded-md bg-blue-700 text-white font-semibold" href="https://www.github.dev/{us.full_name}">Go to Editor</a>
                </li>
                {/each}
                {#if user.length == 0}
                <h1 class="text-2xl font-semibold">No Repositories Available</h1>
                {/if}
            </div>
            <div class="absolute bottom-2 left-0 mx-4 z-20 w-[97%] h-32 bg-gradient-to-t from-white"></div>
        </div>

        {/await}
        {/if}
    </div>
</main>
