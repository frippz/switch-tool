<script>
  import '../styles/base.css';
  import Ports from '../components/Ports/Ports.svelte';
</script>

<div class="switch">
  <Ports switchGroup={1} />
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

  .switch + .switch {
    margin-top: 1.5rem;
  }
</style>
