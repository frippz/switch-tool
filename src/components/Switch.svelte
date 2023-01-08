<script lang="ts">
  import Port from './Port.svelte';

  export let switchGroup: number;
  export let portsPerGroup: number = 8;
  export let numberOfPorts: number;

  // Group the data by 8 ports
  const portGroups: any[][] = [];
  for (let i = 0; i < numberOfPorts; i += portsPerGroup) {
    portGroups.push(
      Array.from({ length: portsPerGroup }, (_, j) => ({
        number: `${i + j + 1}`
      })).slice(0, numberOfPorts - i)
    );
  }

  // Save utilized ports to localStorage
  const savePortState = (portNumber: string, active: boolean) => {
    localStorage.setItem(`switch-${switchGroup}-port-${portNumber}`, active.toString());
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
  for (const group of portGroups) {
    for (const port of group) {
      updatePortState(port.number, port);
    }
  }
</script>

<h2>Switch #{switchGroup} ({numberOfPorts} ports)</h2>
<div class="switch">
  {#each portGroups as group}
    <div class="port-group group-size-{portsPerGroup}">
      {#each group as port}
        <Port {switchGroup} portNumber={port.number} selected={port.selected} {savePortState} />
      {/each}
    </div>
  {/each}
</div>

<style>
  .switch {
    --switch-bg: hsl(0deg 0% 10%);
    --switch-bg-hover: hsl(0deg 0% 14%);

    border-radius: 0.5rem;
    background-color: var(--switch-bg);
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
    --port-size: 2.6em;

    display: grid;
    grid-gap: 0.5rem;
    grid-template-rows: repeat(2, var(--port-size));
    align-items: stretch;
    justify-items: stretch;
    margin-top: 0;
  }

  .port-group.group-size-8 {
    grid-template-columns: repeat(4, var(--port-size));
  }

  .port-group.group-size-12 {
    grid-template-columns: repeat(6, var(--port-size));
  }
</style>
