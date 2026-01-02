<script lang="ts">
  interface Props {
    onSearch: (address: string) => void;
  }

  let { onSearch }: Props = $props();
  let inputValue = $state('');

  function handleSubmit(e: Event) {
    e.preventDefault();
    if (inputValue.trim()) {
      onSearch(inputValue.trim());
    }
  }
</script>

<form class="search-form" onsubmit={handleSubmit}>
  <input
    type="text"
    class="search-input"
    placeholder="Enter address to check flood risk..."
    bind:value={inputValue}
  />
  <button type="submit" class="search-btn">Search</button>
</form>

<style>
  .search-form {
    display: flex;
    flex: 1;
    max-width: 600px;
    gap: 0.5rem;
  }

  .search-input {
    flex: 1;
    padding: 0.625rem 1rem;
    border: 1px solid var(--color-primary);
    border-radius: 6px;
    background: var(--color-bg);
    color: var(--color-text);
    font-size: 0.875rem;
  }

  .search-input::placeholder {
    color: var(--color-text-dim);
  }

  .search-input:focus {
    outline: none;
    border-color: var(--color-accent);
  }

  .search-btn {
    padding: 0.625rem 1.25rem;
    background: var(--color-accent);
    color: white;
    border-radius: 6px;
    font-weight: 500;
    font-size: 0.875rem;
    transition: opacity 0.2s;
  }

  .search-btn:hover {
    opacity: 0.9;
  }
</style>
