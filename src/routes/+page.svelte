<script lang="ts">
  import '../styles/base.css';
  import { onMount } from 'svelte';

  import Switch from '$lib/components/Switch.svelte';
  import AddSwitch from '$lib/components/AddSwitch.svelte';

  let switches: any = [];
  let savedSwitches: any = [];
  let selected: boolean;

  const insertSwitch = () => {
    switches = [...switches, selected];
    localStorage.setItem('switches', JSON.stringify(switches));
    console.log(selected);
  };

  onMount(() => {
    savedSwitches = localStorage.getItem('switches');
    switches = JSON.parse(savedSwitches) || [];
    console.log(switches, selected);
  });
</script>

<h1>Switch Port Utilization Tool™</h1>
<p>Built using Sveltekit by someone that wants to learn more. 😘</p>

<AddSwitch onSubmit={insertSwitch} />

{#each switches as item}
  <Switch numberOfPorts={item} />
{/each}
