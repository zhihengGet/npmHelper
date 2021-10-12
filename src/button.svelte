<script lang="ts">
  import { createEventDispatcher } from "svelte";

  export let value = "";
  export let variant: "info" | "attention" | "must" = "info";
  const dispatch = createEventDispatcher();
  const submit = () => dispatch("submit");

  export let bgColor = "#2b4c56";
  export let color = "aqua";
</script>

<button
  class="myButton"
  style="color:{color};background-color:{bgColor};color:{color};"
  on:click|stopPropagation={submit}><slot>NO INPUT</slot></button
>

<style>
  .myButton {
    border-radius: 28px;
    border: 1px solid #18ab29;
    display: inline-block;
    cursor: pointer;
    font-family: Arial;
    font-size: 17px;
    padding: 10px 20px;
    text-decoration: none;
    text-shadow: 0px 1px 0px #2f6627;
  }
  .myButton:hover {
    background-color: #5cbf2a;
  }
  .myButton:active {
    position: relative;
    top: 1px;
  }
</style>
