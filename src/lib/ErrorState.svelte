<script>
  export let message = "An error occurred";
  export let retryFn = null; // Function to retry the operation
</script>

<div class="error-container">
  <div class="error-icon">⚠️</div>
  <h2>Error</h2>
  <p>{message}</p>
  {#if retryFn}
    <button on:click={retryFn}>Try Again</button>
  {/if}
</div>

<style>
  .error-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    background: rgba(255, 0, 0, 0.05);
    border-radius: 8px;
    max-width: 80%;
  }

  .error-icon {
    font-size: 3rem;
    margin-bottom: 1rem;
  }

  h2 {
    margin: 0 0 0.5rem 0;
    color: #d32f2f;
  }

  p {
    text-align: center;
    margin: 0 0 1rem 0;
  }

  button {
    background: #4980c8;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
  }

  button:hover {
    opacity: 0.9;
  }
</style>