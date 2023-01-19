<script lang="ts" context="module">
  let switchCounter: number = 1;
</script>

<script lang="ts">
  import { v4 as uuidv4 } from 'uuid';
  import { fade } from 'svelte/transition';

  import Port from './Port.svelte';
  import ScrollWrapper from '$lib/components/ScrollWrapper.svelte';

  export let numberOfPorts: number;
  export let componentId: string = uuidv4();

  let portsPerGroup: number;

  if (numberOfPorts >= 24) {
    portsPerGroup = 12;
  } else {
    portsPerGroup = 8;
  }

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
    localStorage.setItem(`switch-${componentId}-port-${portNumber}`, active.toString());
  };

  // Update the state of the port based on the previous state
  const updatePortState = (portNumber: string, port: any) => {
    if (typeof window !== 'undefined') {
      const previousState = localStorage.getItem(`switch-${componentId}-port-${portNumber}`);
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

<section id={componentId} transition:fade>
  <h2>Switch #{switchCounter++} ({numberOfPorts} ports)</h2>
  <ScrollWrapper>
    <div class="switch">
      {#each portGroups as group}
        <div class="port-group group-size-{portsPerGroup}">
          {#each group as port}
            <Port {componentId} portNumber={port.number} selected={port.selected} {savePortState} />
          {/each}
        </div>
      {/each}
    </div>
  </ScrollWrapper>
</section>

<style>
  .switch {
    --switch-bg: hsl(0deg 0% 10%);
    --switch-bg-hover: hsl(0deg 0% 14%);

    border-radius: 0.5rem;
    background-color: var(--switch-bg);
    padding: 1.5rem;
    display: grid;
    grid-gap: 1rem;
    grid-template-columns: repeat(auto-fit, minmax(12rem, min-content));
    align-items: center;
    justify-content: end;
    white-space: nowrap;
    width: 78em;
  }

  .port-group {
    --port-size: 2.6em;

    display: grid;
    grid-gap: 0.5rem;
    grid-template-rows: repeat(2, var(--port-size));
    grid-auto-flow: column;
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
