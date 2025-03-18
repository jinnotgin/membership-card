<script>
  // You can add props for customization if needed
</script>

<div class="loading-container">
  <div class="spinner"></div>
  <p>Loading user information...</p>
</div>

<style>
  .loading-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 2rem;
  }

  .spinner {
    width: 50px;
    height: 50px;
    border: 5px solid rgba(0, 0, 0, 0.1);
    border-radius: 50%;
    border-top-color: var(--color-bgGradient1, #4980c8);
    animation: spin 1s ease-in-out infinite;
    margin-bottom: 1rem;
  }

  @keyframes spin {
    to { transform: rotate(360deg); }
  }

  p {
    font-size: 1.2rem;
    color: var(--color-text, inherit);
  }
</style>