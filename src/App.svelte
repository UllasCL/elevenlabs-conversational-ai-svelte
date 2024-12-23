<script>
  import Button from './lib/Button.svelte';
  import Status from './lib/Status.svelte';

  let isStarted = false;
  let connectionStatus = 'Not Connected';
  let agentStatus = 'Agent Inactive';

  function handleStart() {
    isStarted = true;
    connectionStatus = 'Connected';
    agentStatus = 'Agent Active';
  }

  function handleStop() {
    isStarted = false;
    connectionStatus = 'Not Connected';
    agentStatus = 'Agent Inactive';
  }
</script>

<main>
  <div class="container">
    <h1>SM AI Agent</h1>
    <div class="button-group">
      <Button 
        type="start" 
        disabled={isStarted} 
        onClick={handleStart} 
      />
      <Button 
        type="stop" 
        disabled={!isStarted} 
        onClick={handleStop} 
      />
    </div>
    <Status 
      {connectionStatus}
      {agentStatus}
    />
  </div>
</main>

<style>
  .container {
    background-color: #ffffff;
    padding: 40px;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    max-width: 400px;
    width: 100%;
    text-align: center;
  }

  h1 {
    font-size: 2em;
    margin-bottom: 20px;
    color: #333333;
  }

  .button-group {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-bottom: 30px;
  }
</style>