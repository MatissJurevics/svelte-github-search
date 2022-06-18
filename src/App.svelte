<script lang="ts">
  import axios from 'axios';
  import Searchbar from './components/searchbar.svelte';
  import UserList from './components/userList.svelte';
  import DetailedView from './components/detailedView.svelte';
  const getUsers = async () => {
    try {
      const response = await axios('https://api.github.com/users')
      return response.data
    } catch(err) {
      console.error(err)
    }
  }
  let userList = getUsers()
  let totalCount:number;

  function debounce(func, timeout = 500){
  let timer;
  console.log("debounce")
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => { func.apply(this, args); }, timeout);
  };
}

  const handleSearch = async e => {
    console.log("test");
    try {
      const res = await axios(`https://api.github.com/search/users?q=${e.detail}`)
      console.log(res.data)
      userList = res.data.items
      totalCount = res.data.total_count
    } catch (err) {console.log(err)}
  }
  $: searchCount = totalCount === undefined ? 46 : totalCount 
  let selectedUser;
  let detailedView = false 
  const exitDetailedView = () => detailedView = false
  const enterDetailedView = e => {
    selectedUser = e.detail
    detailedView = true
  }

  const processChanges = debounce(e => handleSearch(e))

</script>

<main class="w-screen min-h-screen bg-slate-700 flex flex-col items-center">
  <h1 class="text-3xl font-bold text-white p-3">Github Account Search</h1>
  {#await userList}
    <p>Waiting...</p>
  {:then userList}
  <Searchbar on:target-change={processChanges} />
  <h1 class="text-xl font-semibold text-white">{searchCount} Total Results</h1>
  <UserList on:detailed-view={enterDetailedView} {userList} />
  {:catch}
  <h1 class="text-red-200 text-3xl">Cannot access API</h1>
  {/await}
  {#if detailedView}
    <DetailedView on={detailedView} userData={selectedUser} on:exit-selected={exitDetailedView} />
  {/if}
</main>