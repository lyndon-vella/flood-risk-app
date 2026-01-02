<script lang="ts">
  interface PropertyData {
    address: string;
    elevation: number;
    riskLevel: 'safe' | 'low' | 'medium' | 'high';
  }

  interface Props {
    propertyData: PropertyData | null;
    seaLevel: number;
  }

  let { propertyData, seaLevel }: Props = $props();

  const riskConfig = {
    safe: { color: 'var(--color-safe)', label: 'Safe', icon: '✓' },
    low: { color: 'var(--color-warning)', label: 'Low Risk', icon: '!' },
    medium: { color: '#f97316', label: 'Medium Risk', icon: '!!' },
    high: { color: 'var(--color-danger)', label: 'High Risk', icon: '⚠' }
  };

  function getAdvice(riskLevel: string): string[] {
    switch (riskLevel) {
      case 'safe':
        return ['Your property is currently at low flood risk.', 'Consider flood insurance for future protection.'];
      case 'low':
        return [
          'Minor flooding possible at projected sea levels.',
          'Review your flood insurance coverage.',
          'Consider basic flood preparedness measures.'
        ];
      case 'medium':
        return [
          'Significant flooding likely at projected sea levels.',
          'Flood insurance is strongly recommended.',
          'Create an emergency evacuation plan.',
          'Consider flood mitigation improvements.'
        ];
      case 'high':
        return [
          'Severe flooding expected at projected sea levels.',
          'Flood insurance is essential.',
          'Prepare emergency supplies and evacuation routes.',
          'Consult with flood mitigation specialists.',
          'Consider long-term relocation planning.'
        ];
      default:
        return [];
    }
  }
</script>

<aside class="side-panel">
  {#if propertyData}
    <div class="panel-section">
      <h2 class="section-title">Property Info</h2>
      <div class="info-row">
        <span class="info-label">Address</span>
        <span class="info-value">{propertyData.address}</span>
      </div>
      <div class="info-row">
        <span class="info-label">Elevation</span>
        <span class="info-value">{propertyData.elevation} ft</span>
      </div>
      <div class="info-row">
        <span class="info-label">Sea Level Rise</span>
        <span class="info-value">{seaLevel} ft</span>
      </div>
    </div>

    <div class="panel-section">
      <h2 class="section-title">Risk Assessment</h2>
      <div
        class="risk-badge"
        style="background: {riskConfig[propertyData.riskLevel].color}"
      >
        <span class="risk-icon">{riskConfig[propertyData.riskLevel].icon}</span>
        <span class="risk-label">{riskConfig[propertyData.riskLevel].label}</span>
      </div>
    </div>

    <div class="panel-section">
      <h2 class="section-title">Recommendations</h2>
      <ul class="advice-list">
        {#each getAdvice(propertyData.riskLevel) as advice}
          <li class="advice-item">{advice}</li>
        {/each}
      </ul>
    </div>
  {:else}
    <div class="empty-state">
      <div class="empty-icon">🏠</div>
      <p class="empty-text">Search for an address to see flood risk assessment</p>
    </div>
  {/if}
</aside>

<style>
  .side-panel {
    width: var(--sidebar-width);
    background: var(--color-surface);
    border-left: 1px solid var(--color-primary);
    overflow-y: auto;
    padding: 1.5rem;
  }

  .panel-section {
    margin-bottom: 1.5rem;
  }

  .section-title {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    color: var(--color-text-dim);
    margin-bottom: 0.75rem;
  }

  .info-row {
    display: flex;
    justify-content: space-between;
    padding: 0.5rem 0;
    border-bottom: 1px solid var(--color-primary);
  }

  .info-label {
    color: var(--color-text-dim);
    font-size: 0.875rem;
  }

  .info-value {
    font-weight: 500;
    font-size: 0.875rem;
  }

  .risk-badge {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.75rem 1rem;
    border-radius: 8px;
    color: white;
    font-weight: 600;
  }

  .risk-icon {
    font-size: 1.25rem;
  }

  .advice-list {
    list-style: none;
  }

  .advice-item {
    padding: 0.5rem 0;
    padding-left: 1rem;
    position: relative;
    font-size: 0.875rem;
    color: var(--color-text);
    line-height: 1.4;
  }

  .advice-item::before {
    content: '→';
    position: absolute;
    left: 0;
    color: var(--color-accent);
  }

  .empty-state {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
    text-align: center;
    color: var(--color-text-dim);
  }

  .empty-icon {
    font-size: 3rem;
    margin-bottom: 1rem;
    opacity: 0.5;
  }

  .empty-text {
    font-size: 0.875rem;
    max-width: 200px;
  }
</style>
