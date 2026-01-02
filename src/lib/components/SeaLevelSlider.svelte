<script lang="ts">
  interface Props {
    value: number;
  }

  let { value = $bindable(0) }: Props = $props();

  function getRiskColor(level: number): string {
    if (level <= 1) return 'var(--color-safe)';
    if (level <= 3) return 'var(--color-warning)';
    return 'var(--color-danger)';
  }
</script>

<div class="slider-card">
  <div class="slider-header">
    <span class="slider-label">Sea Level Rise</span>
    <span class="slider-value" style="color: {getRiskColor(value)}">{value} ft</span>
  </div>
  <input
    type="range"
    class="slider"
    min="0"
    max="10"
    step="0.5"
    bind:value
    style="--progress: {(value / 10) * 100}%; --thumb-color: {getRiskColor(value)}"
  />
  <div class="slider-labels">
    <span>0 ft</span>
    <span>10 ft</span>
  </div>
</div>

<style>
  .slider-card {
    background: var(--color-surface);
    padding: 1rem 1.5rem;
    border-radius: 12px;
    min-width: 280px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
    border: 1px solid var(--color-primary);
  }

  .slider-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.75rem;
  }

  .slider-label {
    font-size: 0.875rem;
    color: var(--color-text-dim);
  }

  .slider-value {
    font-size: 1.25rem;
    font-weight: 600;
  }

  .slider {
    width: 100%;
    height: 8px;
    border-radius: 4px;
    background: linear-gradient(
      to right,
      var(--thumb-color) 0%,
      var(--thumb-color) var(--progress),
      var(--color-bg) var(--progress),
      var(--color-bg) 100%
    );
    appearance: none;
    cursor: pointer;
  }

  .slider::-webkit-slider-thumb {
    appearance: none;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: var(--thumb-color);
    border: 3px solid white;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
    cursor: grab;
  }

  .slider::-webkit-slider-thumb:active {
    cursor: grabbing;
  }

  .slider-labels {
    display: flex;
    justify-content: space-between;
    margin-top: 0.5rem;
    font-size: 0.75rem;
    color: var(--color-text-dim);
  }
</style>
