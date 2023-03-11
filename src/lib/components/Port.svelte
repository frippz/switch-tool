<script lang="ts">
  export let componentId: string;
  export let portNumber: string;
  export let selected: boolean;
  export let savePortState: (portNumber: string, active: boolean) => void;
</script>

<div class="port">
  <input
    type="checkbox"
    id="{componentId}-port-{portNumber}"
    bind:checked={selected}
    on:change={() => savePortState(portNumber, selected)}
  />
  <label for="{componentId}-port-{portNumber}">{portNumber}</label>
</div>

<style>
  .port {
    --port-bg: hsl(0deg 0% 20%);
    --port-active: hsla(125deg 33% 50% / 190%);

    display: flex;
    margin-top: 0;
    font-size: 0.8em;
  }

  input {
    position: absolute;
    top: -1rem;
    left: -1rem;
  }

  label {
    position: relative;
    display: flex;
    flex: 1;
    align-items: end;
    justify-content: start;
    padding: 0.25rem;
    margin-top: 0;
    overflow: hidden;
    cursor: pointer;
    background-color: var(--port-bg);
    border-radius: 0.25rem;
  }

  label:hover {
    filter: brightness(1.2);
  }

  input:checked + label {
    background-color: var(--port-active);
  }

  input:focus-visible + label {
    outline: none;
    box-shadow: 0 0 0 0.25em var(--switch-bg), 0 0 0 0.375em var(--port-active);
    transition: box-shadow 0.2s ease-out;
  }
</style>
