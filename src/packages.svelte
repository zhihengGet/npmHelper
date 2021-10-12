<script lang="ts">
  export let info: {
    name: string;
    github: string;
    npm: string;
    createdAt: string;
    updatedAt: string;
  } = {};
  import Button from "./button.svelte";
</script>

<div>
  <emphaize>{info.name}</emphaize>
</div>

<style>
  .package {
    width: 500px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  }
</style>
