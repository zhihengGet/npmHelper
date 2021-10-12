<script lang="ts">
  import { debug } from "svelte/internal";
  import Button from "./button.svelte";
  import CMD from "./cmd.svelte";
  import PackageList from "./packages.svelte";

  import {
    QueryClient,
    QueryClientProvider,
    useQuery,
  } from "@sveltestack/svelte-query";

  let useNPM = true;
  let text = "";
  let prefix = useNPM ? "npm i " : "yarn add ";
  let link = "";
  //let result: any = {};

  async function search() {
    console.log("search");

    const r = await fetch(
      "https://api.npms.io/v2/search?size=10&q=" + new URLSearchParams(text)
    );
    return await r.json();
  }

  function toggle() {
    useNPM = !useNPM;
  } //App.svelte

  // Create a client
</script>

<QueryClientProvider client={queryClient}>
  <div class="container">
    <div class="searchContainer">
      <h1 class="title">Install Utility</h1>
      <input
        type="search"
        class="search"
        placeholder="search..."
        bind:value={text}
      />
      <Button style={{ text: "submit" }} on:click={search}>submit</Button>
    </div>

    <div>
      <label for="npm" class="npm_select">npm</label>
      <input id="npm" type="checkbox" checked={useNPM} on:click={toggle} />
      <label for="yarn">yarn</label>
      <input
        id="yarn"
        type="checkbox"
        checked={useNPM == false}
        on:click={toggle}
      />
    </div>
    <CMD />
  </div>
  {#if $result.isLoading}
    <span>Loading...</span>
  {:else if $result.isError}
    <span>Error: {$result.error}</span>
  {:else}
    <ul>
      {#each $result.data as data}
        <li>{data.package.name}</li>
      {/each}
    </ul>
  {/if}
</QueryClientProvider>

<style>
  .container {
    width: 80%;
    height: 600px;
    margin: auto;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  label {
    color: aqua;
    vertical-align: middle;
  }
  .title {
    font-size: 3em;
    font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  }
  .searchContainer {
    display: flex;
    align-items: center;
    flex-direction: column;
  }
  .search {
    width: 400px;
    border-radius: 30px;
    padding: 10px;
    padding-left: 1em;
    color: aquamarine;
    font-size: larger;
    background-color: #256858;
    margin: 1em;
    outline: none;
    border: solid 1px gray;
  }
  :global(body) {
    margin: 0;
    background: linear-gradient(-45deg, #7c3b26, #6a3c4e, #224652, #256858);
    background-size: 400% 400%;
    animation: gradient 60s ease infinite;
    height: 100vh;
  }

  @keyframes gradient {
    0% {
      background-position: 0% 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }

  @media only screen and (max-width: 800px) {
    .container {
      width: 100vw;
    }
    .searchContainer,
    .search {
      width: 80%;
    }
  }
</style>
