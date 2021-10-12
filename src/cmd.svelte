<script>
  import Button from "./button.svelte";

  export let ref;
  export let cmd = "empty";
  let helper = "copy";
  export function focus() {
    ref.focus();
  }
</script>

<textarea class="cmd" value={cmd} bind:this={ref} />

<Button
  on:submit={() => {
    helper = "* copied *";
    navigator.clipboard.writeText(cmd);
    setTimeout(() => {
      helper = "copy";
    }, 5000);
  }}>{helper}</Button
>

<style>
  .cmd {
    margin: 1em;
    height: 50px;
    width: 200px;
    background-color: rgb(148, 221, 155);
    border: solid 1px;
    font-size: larger;
    padding-left: 5px;
    font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
      "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  }
</style>
