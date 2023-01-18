<script lang="ts">
  import '../styles/base.css';
  import { onMount } from 'svelte';

  import Switch from '$lib/components/Switch.svelte';

  let switches: any = [];
  let savedSwitches: any = [];
  let selected: boolean;

  onMount(() => {
    savedSwitches = localStorage.getItem('switches');
    switches = JSON.parse(savedSwitches) || [];
  });

  const insertSwitch = () => {
    switches = [...switches, selected];
    localStorage.setItem('switches', JSON.stringify(switches));
    console.log(selected);
  };
</script>

<h1>Switch Port Utilization Tool™</h1>
<p>Built using Sveltekit by someone that wants to learn more. 😘</p>

<form on:submit|preventDefault={insertSwitch}>
  <select id="switchsize" bind:value={selected}>
    <option value="16">16</option>
    <option value="24">24</option>
    <option value="48">48</option>
  </select>
  <button type="submit">Add switch</button>
</form>

{#each switches as item}
  <Switch numberOfPorts={item} />
{/each}
