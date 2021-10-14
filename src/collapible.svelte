<script lang="ts">
  type packageType = {
    package: {
      name: string;
      scope: "unscoped" | "scoped";
      version: string;
      description: string;
      keywords: [];
      date: "2021-07-19T20:34:33.914Z";
      links: {
        npm: "https://www.npmjs.com/package/passport-azure-ad";
        homepage: "https://github.com/AzureAD/microsoft-authentication-library-for-js#readme";
        repository: "https://github.com/AzureAD/microsoft-authentication-library-for-js";
        bugs: "https://github.com/AzureAD/microsoft-authentication-library-for-js/issues";
      };
      author: {
        name: "azuread";
        email: "nugetaad@microsoft.com";
        url: "http://microsoft.com/";
        username: "azuread";
      };
      publisher: {
        username: "azuread";
        email: "nugetaad@microsoft.com";
      };
      maintainers: [
        {
          username: "msopentech";
          email: "msopentech@microsoft.com";
        },
        {
          username: "azuread";
          email: "nugetaad@microsoft.com";
        }
      ];
    };
    score: {
      final: 0.6971885108315556;
      detail: {
        quality: 0.8047911709983551;
        popularity: 0.3093410680942503;
        maintenance: 0.9928051019973188;
      };
    };
    searchScore: 0.38966373;
  };

  import { fade } from "svelte/transition";
  import { createEventDispatcher } from "svelte";
  import Button from "./button.svelte";

  const dispatch = createEventDispatcher();
  //type npm={total:Number,results:}
  export let data: packageType = {};
  export let selected;

  $: console.log(selected);
  let show = false;
</script>

<div
  class="container "
  style={selected.has(data.package.name) ? "border-left:solid 10px purple" : ""}
>
  <span>
    <div
      class="collapsible"
      on:click={() => {
        show = !show;
      }}
    >
      <span>
        <a
          href={data.package.links.npm}
          style="position: relative;"
          target="_blank"
        >
          <span class="truncate name">
            {data.package.name}
            <span class="tooltip">
              <span class="arrow-down" />{data.package.name}</span
            >
          </span>
        </a>
      </span>

      <a href={data.package.links.repository} target="_blank"
        >v{data.package.version}</a
      >

      <a href={data.package.links.homepage} target="_blank">
        <span style="vertical-align: middle">Github</span>
      </a>
      <img src="github.png" style="margin-left:10px" alt="?" class="git" />
      <p>
        Published:{new Date(data.package.date).toLocaleDateString("us")}
      </p>
      <p>relevance score:{data.score?.final.toFixed(3)}</p>
      <Button
        variant="must"
        on:submit={() => {
          let n = data.package.name; //data.package.name + "@" + data.package.version;
          if (!selected.has(n)) selected.add(n);
          else selected.delete(n);
          selected = selected;
        }}>{selected.has(data.package.name) ? "remove" : "add"}</Button
      >
    </div>

    <div class={show ? "content2" : "content"} transition:fade>
      <h5>{data.package.name}</h5>
      <p>
        <b> Info: </b>
        {data.package.description}
      </p>
      <p>
        <em>Maintainer </em> : {data.package.maintainers
          .map((m) => m.username)
          .join(", ")}
      </p>
      <p><b>Publisher:</b> {data.package.publisher.username}</p>
    </div>
  </span>
</div>

<style>
  .truncate {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  p {
    padding-left: 1em;
  }
  .collapsible {
    background-color: rgb(28, 124, 79);
    color: white;
    padding: 1em;
    width: 100vw;
    outline: none;
    font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
      "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
    color: antiquewhite;
    display: inline-flex;
    flex-wrap: wrap;

    align-items: center;
  }
  .name:hover .tooltip {
    position: absolute;
    left: 0;
    background-color: black;
    top: -60px;
    display: inline;
    padding: 0.6em;
    border: solid 1px #81e82c;
    text-align: start;
  }
  .arrow-down {
    width: 0;
    height: 0;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-top: 14px solid #515151;
    position: absolute;
    bottom: -14px;
    color: black;
  }

  .tooltip {
    display: none;
  }
  .name {
    color: #81e82c;
    font-weight: 900;
    text-overflow: ellipsis;
  }

  a,
  .name {
    max-width: 19em;
  }

  .collapsible:hover {
    background-color: green;
  }
  a {
    color: yellow;
    padding-left: 1em;
  }
  .container {
    margin-bottom: 1em;
    width: calc(500px-2em);
    max-width: 100vw;
    display: flex;
    animation-name: rotate-in-2-cw;
    animation-duration: 4s;
  }
  .content {
    display: none;
  }
  .content2 {
    text-overflow: ellipsis;
    background-color: #f1f1f1;
    padding: 1em;
    text-transform: capitalize;
  }
  .git {
    width: 1em;
    height: 1em;
    vertical-align: super;
  }
  @media only screen and (min-width: 550px) {
    .container,
    .collapsible {
      width: 400px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    .collapsible {
      flex-direction: row;
    }
    .git {
      width: 1.5em;
      height: 1.5em;
      vertical-align: auto;
    }
    a,
    .name {
      max-width: 100px;
    }
  }
  @media only screen and (min-width: 900px) {
    .container,
    .collapsible {
      width: 800px;
    }
    .collapsible {
      flex-direction: row;
    }
    .git {
      width: 1.5em;
      height: 1.5em;
      vertical-align: auto;
    }
    a,
    .name {
      max-width: 150px;
    }
  }
</style>
