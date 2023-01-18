<script lang="ts">
  export let onSubmit: (selected: number) => void;

  let selected: number;

  const submitSwitchConfig = () => {
    onSubmit(selected);
  };
</script>

<form on:submit|preventDefault={submitSwitchConfig}>
  <select id="switchsize" bind:value={selected}>
    <option value="16">16</option>
    <option value="24">24</option>
    <option value="48">48</option>
  </select>
  <button type="submit">Add switch</button>
</form>
