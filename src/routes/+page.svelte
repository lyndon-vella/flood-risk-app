<script lang="ts">
  import { onMount } from 'svelte';
  import AddressSearch from '$lib/components/AddressSearch.svelte';
  import SeaLevelSlider from '$lib/components/SeaLevelSlider.svelte';
  import SidePanel from '$lib/components/SidePanel.svelte';
  import CesiumViewer from '$lib/components/CesiumViewer.svelte';

  let seaLevel = $state(0);
  let viewer: any = $state(null);
  let Cesium: any = $state(null);
  let mounted = $state(false);
  let propertyInfo = $state<{
    address: string;
    elevation: number;
  } | null>(null);

  // Derive risk level from sea level and elevation
  function getRiskLevel(elevation: number, seaLevelRise: number): 'safe' | 'low' | 'medium' | 'high' {
    const margin = elevation - seaLevelRise;
    if (margin > 10) return 'safe';
    if (margin > 5) return 'low';
    if (margin > 0) return 'medium';
    return 'high';
  }

  // Computed property data with current risk level
  let propertyData = $derived(propertyInfo ? {
    ...propertyInfo,
    riskLevel: getRiskLevel(propertyInfo.elevation, seaLevel)
  } : null);

  onMount(async () => {
    Cesium = await import('cesium');
    mounted = true;
  });

  function handleViewerReady(v: any) {
    viewer = v;
  }

  async function handleSearch(searchAddress: string) {
    // Geocode the address using Nominatim
    try {
      const response = await fetch(
        `https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(searchAddress)}`
      );
      const results = await response.json();

      if (results.length > 0 && viewer && Cesium) {
        const { lat, lon } = results[0];
        const longitude = parseFloat(lon);
        const latitude = parseFloat(lat);

        // Fly to the location
        viewer.camera.flyTo({
          destination: Cesium.Cartesian3.fromDegrees(longitude, latitude, 800),
          orientation: {
            heading: Cesium.Math.toRadians(0),
            pitch: Cesium.Math.toRadians(-45),
            roll: 0
          },
          duration: 2
        });

        // Mock elevation data for now
        propertyInfo = {
          address: results[0].display_name,
          elevation: Math.floor(Math.random() * 20) + 5
        };
      }
    } catch (error) {
      console.error('Geocoding failed:', error);
    }
  }
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
      {#if mounted}
        <CesiumViewer onReady={handleViewerReady} />
      {:else}
        <div class="map-loading">
          <p>Loading 3D Map...</p>
        </div>
      {/if}
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

  .map-loading {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--color-bg);
    color: var(--color-text-dim);
    font-size: 1.25rem;
  }

  .slider-container {
    position: absolute;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
  }
</style>
