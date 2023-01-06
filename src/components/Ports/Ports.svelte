<script lang="ts">
  import data from '../../data/switch.json';

  export let switchGroup: number;

  // Group the data by 8 ports
  const groupedData: any[][] = [];
  for (let i = 0; i < data.length; i += 8) {
    groupedData.push(data.slice(i, i + 8));
  }

  // Save utilized ports to localStorage
  const savePortState = (portNumber: string, active: string) => {
    localStorage.setItem(`port-${portNumber}`, active);
  };
</script>

{#each groupedData as group}
  <div class="port-group">
    {#each group as port}
      <div class="port">
        <input
          type="checkbox"
          id="sw{switchGroup}-port-{port.number}"
          bind:checked={port.selected}
          on:change={() => savePortState(port.number, port.selected)}
        />
        <label for="sw1-port-{port.number}">{port.number}</label>
      </div>
    {/each}
  </div>
{/each}

<style>
  .port-group {
    display: grid;
    grid-gap: 0.5rem;
    grid-template: repeat(2, 3em) / repeat(4, 3em);
    align-items: stretch;
    justify-items: stretch;
  }

  .port {
    display: flex;
  }

  input {
    position: absolute;
    top: -1rem;
    left: -1rem;
  }

  label {
    flex: 1;
    display: flex;
    align-items: end;
    justify-content: start;
    overflow: hidden;
    border-radius: 0.25rem;
    background-color: hsla(0deg 0% 100% / 0.15);
    position: relative;
    padding: 0.25rem;
  }

  input:checked + label {
    background-color: hsla(125deg 33% 50% / 1.9);
  }
</style>
