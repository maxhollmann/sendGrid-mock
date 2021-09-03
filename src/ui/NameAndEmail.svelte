<script>
  export let data;
</script>

{#if data.name}
  {data.name} &lt;{data.email}&gt;
{:else}
  {data.email}
{/if}
