<script lang="ts">
  import AddressSearch from '$lib/components/AddressSearch.svelte';
  import SeaLevelSlider from '$lib/components/SeaLevelSlider.svelte';
  import SidePanel from '$lib/components/SidePanel.svelte';

  let seaLevel = $state(0);
  let address = $state('');
  let propertyData = $state<{
    address: string;
    elevation: number;
    riskLevel: 'safe' | 'low' | 'medium' | 'high';
  } | null>(null);

  function handleSearch(searchAddress: string) {
    address = searchAddress;
    // Mock data for UI demo
    propertyData = {
      address: searchAddress,
      elevation: 12,
      riskLevel: seaLevel > 6 ? 'high' : seaLevel > 3 ? 'medium' : seaLevel > 1 ? 'low' : 'safe'
    };
  }

  // Update risk level when sea level changes
  $effect(() => {
    if (propertyData) {
      propertyData = {
        ...propertyData,
        riskLevel: seaLevel > 6 ? 'high' : seaLevel > 3 ? 'medium' : seaLevel > 1 ? 'low' : 'safe'
      };
    }
  });
</script>

<div class="app">
  <header class="header">
    <div class="logo">
      <span class="logo-icon">🌊</span>
      <span class="logo-text">FloodRisk</span>
    </div>
    <AddressSearch onSearch={handleSearch} />
  </header>

  <main class="main">
    <div class="map-container">
      <div class="map-placeholder">
        <p>3D Map</p>
        <p class="map-hint">Cesium viewer will render here</p>
      </div>
      <div class="slider-container">
        <SeaLevelSlider bind:value={seaLevel} />
      </div>
    </div>
    <SidePanel {propertyData} {seaLevel} />
  </main>
</div>

<style>
  .app {
    display: flex;
    flex-direction: column;
    height: 100vh;
  }

  .header {
    height: var(--header-height);
    background: var(--color-surface);
    display: flex;
    align-items: center;
    padding: 0 1rem;
    gap: 1rem;
    border-bottom: 1px solid var(--color-primary);
  }

  .logo {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-weight: 600;
    font-size: 1.25rem;
  }

  .logo-icon {
    font-size: 1.5rem;
  }

  .main {
    display: flex;
    flex: 1;
    overflow: hidden;
  }

  .map-container {
    flex: 1;
    position: relative;
    background: var(--color-bg);
  }

  .map-placeholder {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, var(--color-surface) 0%, var(--color-bg) 100%);
    color: var(--color-text-dim);
  }

  .map-placeholder p {
    font-size: 1.5rem;
  }

  .map-hint {
    font-size: 0.875rem !important;
    margin-top: 0.5rem;
  }

  .slider-container {
    position: absolute;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
  }
</style>
