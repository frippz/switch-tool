<script lang="ts">
  export let switchGroup: number;
  export let portsPerGroup: number = 8;
  export let numPorts: number;

  // Group the data by 8 ports
  const groupedData: any[][] = [];
  Array.from({ length: Math.ceil(numPorts / portsPerGroup) }).forEach((_, i) => {
    groupedData.push(
      Array.from({ length: portsPerGroup }, (_, j) => ({
        number: `${i * portsPerGroup + j + 1}`
      })).slice(0, numPorts - i * portsPerGroup)
    );
  });

  // Save utilized ports to localStorage
  const savePortState = (portNumber: string, active: string) => {
    localStorage.setItem(`switch-${switchGroup}-port-${portNumber}`, active);
  };

  // Update the state of the port based on the previous state
  const updatePortState = (portNumber: string, port: any) => {
    if (typeof window !== 'undefined') {
      const previousState = localStorage.getItem(`switch-${switchGroup}-port-${portNumber}`);
      if (previousState !== null) {
        port.selected = previousState === 'true';
      }
    }
  };

  // Update the state of each port
  for (const group of groupedData) {
    for (const port of group) {
      updatePortState(port.number, port);
    }
  }
</script>

<h2>Switch #{switchGroup} ({numPorts} ports)</h2>
<div class="switch">
  {#each groupedData as group}
    <div class="port-group group-size-{portsPerGroup}">
      {#each group as port}
        <div class="port">
          <input
            type="checkbox"
            id="sw{switchGroup}-port-{port.number}"
            bind:checked={port.selected}
            on:change={() => savePortState(port.number, port.selected)}
          />
          <label for="sw{switchGroup}-port-{port.number}">{port.number}</label>
        </div>
      {/each}
    </div>
  {/each}
</div>

<style>
  .switch {
    border-radius: 0.5rem;
    background-color: hsl(0deg 0% 10%);
    height: 10rem;
    padding: 1rem;
    display: grid;
    grid-gap: 1rem;
    grid-template-columns: repeat(auto-fit, minmax(12rem, min-content));
    align-items: center;
    justify-content: end;
    white-space: nowrap;
  }

  .port-group {
    display: grid;
    grid-gap: 0.5rem;
    grid-template-rows: repeat(2, 3em);
    align-items: stretch;
    justify-items: stretch;
    margin-top: 0;
  }

  .port-group.group-size-8 {
    grid-template-columns: repeat(4, 3em);
  }

  .port-group.group-size-12 {
    grid-template-columns: repeat(6, 3em);
  }

  .port {
    display: flex;
    margin-top: 0;
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
    margin-top: 0;
  }

  input:checked + label {
    background-color: hsla(125deg 33% 50% / 1.9);
  }
</style>
