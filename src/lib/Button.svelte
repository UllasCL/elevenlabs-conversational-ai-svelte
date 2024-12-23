<script>
  export let type = 'start';
  export let disabled = false;
  export let onClick = () => {};

  $: buttonClass = type === 'start' ? 'start' : 'stop';
</script>

<button
  class={buttonClass}
  on:click={onClick}
  {disabled}
  aria-label={`${type === 'start' ? 'Start' : 'Stop'} Conversation`}
>
  {type === 'start' ? 'Start' : 'Stop'} Conversation
</button>

<style>
  button {
    flex: 1;
    padding: 12px 20px;
    font-size: 1em;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .start {
    background-color: #4CAF50;
    color: white;
  }

  .start:hover:not(:disabled) {
    background-color: #45a049;
  }

  .stop {
    background-color: #f44336;
    color: white;
  }

  .stop:hover:not(:disabled) {
    background-color: #da190b;
  }

  button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
  }
</style>