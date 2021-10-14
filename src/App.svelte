<script lang="ts">
  import { debug, tick } from "svelte/internal";
  import Button from "./button.svelte";
  import CMD from "./cmd.svelte";
  import PackageList from "./packages.svelte";
  // Gray 100 theme
  import {
    QueryClient,
    QueryClientProvider,
    useQuery,
  } from "@sveltestack/svelte-query";
  import Collapible from "./collapible.svelte";

  let useNPM = true;
  let text = "";
  let prefix = useNPM ? "npm i " : "yarn add ";
  $: prefix = useNPM ? "npm i " : "yarn add ";
  let link = "";

  let result: any = {};
  const queryClient = new QueryClient();
  let selected = new Set();
  let withVersionSelected = new Set();
  let cmd = "";
  let useVersion = false;

  $: {
    cmd = prefix + Array.from(selected).join(" ");
  }
  let searchRef;
  /*   $: {
    console.log(selected, " sdf");
    (async function t() {
      await tick();
      ref.focus();
    })();
  } */

  async function search() {
    console.log("search");
    const r = await queryClient.fetchQuery(["search", text], async (e) => {
      const r = await fetch(
        "https://api.npms.io/v2/search?size=20&q=" + new URLSearchParams(text)
      );
      return r.json();
    });
    console.log(r);
    result = r;
  }
  let ref;

  function toggle() {
    useNPM = !useNPM;
  } //App.svelte

  function onEnter(e) {
    console.log("keydown", e.code == "Enter");
    if (e.code == "Enter") search();
  }

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
        on:keydown={onEnter}
      />
      <Button
        style={{ text: "submit" }}
        bind:this={searchRef}
        on:submit={search}>submit</Button
      >
    </div>

    <div
      style="display:flex;justify-content:center;flex-direction:column;align-items:center"
    >
      <span>
        <label for="npm" class="npm_select">npm</label>
        <input id="npm" type="checkbox" checked={useNPM} on:click={toggle} />
        <label for="yarn">yarn</label>
        <input
          id="yarn"
          type="checkbox"
          checked={useNPM == false}
          on:click={toggle}
        />
      </span>
      <CMD {cmd} bind:this={ref} />
    </div>

    <div class="badges">
      <!-- content here -->
      {#each Array.from(selected) as item}
        <!-- content here -->
        <div
          class="badge"
          on:click={() => {
            selected.delete(item);
            selected = selected;
          }}
        >
          {item}
        </div>
      {/each}
    </div>

    <div>
      {#await result}
        <!-- results is pending -->
        <p>loading...</p>
      {:then value}
        <!-- results was fulfilled -->
        {#if result && Array.isArray(result.results)}
          <ol class="package">
            {#each result.results as data}
              <li><Collapible {data} bind:selected /></li>
            {/each}
          </ol>
          {#if result.results.length == 0}
            <h4 style="color:lightyellow">Empty</h4>
          {/if}
        {/if}
      {:catch error}
        <!-- results was rejected -->
        <p>error occurred{error}</p>
      {/await}
    </div>

    <footer style="padding:1em">
      Made with <a
        href="https://npms.io/"
        style="color: aquamarine;text-transform:lowercase;">npms.io</a
      > and love.
    </footer>
  </div>
</QueryClientProvider>

<style>
  .container {
    width: 80%;
    min-height: 600px;
    margin: auto;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  ol,
  li {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }
  footer {
    color: antiquewhite;
  }
  .badges {
  }
  .badge {
    width: 10em;
    border: solid 1px purple;
    background-color: yellow;
    margin: 10px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    padding: 4px;
    font-weight: 800;
    transition: cubic-bezier(0.075, 0.82, 0.165, 1);
  }
  label {
    color: aqua;
    vertical-align: middle;
  }
  input[type="checkbox"] {
    vertical-align: bottom;
    background-color: blueviolet;
    font-size: 2em;
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
    text-transform: capitalize;
  }
</style>
