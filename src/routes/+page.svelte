<script>
  import '../styles/base.css';
  import Switch from '../components/Switch.svelte';
</script>

<h1>Switch Port Utilization Tool™</h1>
<p>Built using Sveltekit by someone that wants to learn more. 😘</p>

<Switch switchGroup={1} numberOfPorts={16} portsPerGroup={8} />

<Switch switchGroup={2} numberOfPorts={24} />
