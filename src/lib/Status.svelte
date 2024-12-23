<script>
  export let connectionStatus = '';
  export let agentStatus = '';
</script>

<div class="status">
  <p><span>{connectionStatus}</span></p>
  <p><span>{agentStatus}</span></p>
</div>

<style>
  .status {
    font-size: 1.1em;
    color: #555555;
    margin-bottom: 10px;
  }

  span {
    font-weight: bold;
    color: #000000;
  }
</style>