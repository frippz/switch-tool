<script lang="ts">
  export let switchGroup: number;
  export let portNumber: string;
  export let selected: boolean;
  export let savePortState: (portNumber: string, active: boolean) => void;
</script>

<div class="port">
  <input
    type="checkbox"
    id="sw{switchGroup}-port-{portNumber}"
    bind:checked={selected}
    on:change={() => savePortState(portNumber, selected)}
  />
  <label for="sw{switchGroup}-port-{portNumber}">{portNumber}</label>
</div>

<style>
  .port {
    --port-bg: hsl(0deg 0% 20%);
    --port-active: hsla(125deg 33% 50% / 1.9);

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
    flex: 1;
    display: flex;
    align-items: end;
    justify-content: start;
    overflow: hidden;
    border-radius: 0.25rem;
    background-color: var(--port-bg);
    position: relative;
    padding: 0.25rem;
    margin-top: 0;
    cursor: pointer;
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
