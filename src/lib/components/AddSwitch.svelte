<script lang="ts">
  export let onSubmit: (ports: number) => void;

  let ports: number;

  const submitSwitchConfig = () => {
    onSubmit(ports);
  };
</script>

<form on:submit|preventDefault={submitSwitchConfig}>
  <select id="switchsize" bind:value={ports}>
    <option value="16">16</option>
    <option value="24">24</option>
    <option value="48">48</option>
  </select>
  <button type="submit">Add switch</button>
</form>
