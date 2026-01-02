<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  interface Props {
    onReady?: (viewer: any) => void;
  }

  let { onReady }: Props = $props();

  let container: HTMLDivElement;
  let viewer: any = null;
  let error = $state<string | null>(null);

  onMount(async () => {
    try {
      // Dynamic import of Cesium
      const Cesium = await import('cesium');

      // Import CSS
      await import('cesium/Build/Cesium/Widgets/widgets.css');

      // Set Cesium Ion access token
      // Get your FREE token at: https://cesium.com/ion/tokens
      // 1. Create account at cesium.com/ion
      // 2. Go to Access Tokens
      // 3. Copy your default token and paste below
      const CESIUM_TOKEN = import.meta.env.VITE_CESIUM_TOKEN || '';

      if (!CESIUM_TOKEN) {
        throw new Error('Missing Cesium Ion token. Add VITE_CESIUM_TOKEN to .env file');
      }

      Cesium.Ion.defaultAccessToken = CESIUM_TOKEN;

      // Create the Cesium Viewer
      viewer = new Cesium.Viewer(container, {
        terrain: Cesium.Terrain.fromWorldTerrain(),
        animation: false,
        baseLayerPicker: false,
        fullscreenButton: false,
        geocoder: false,
        homeButton: false,
        infoBox: false,
        sceneModePicker: false,
        selectionIndicator: false,
        timeline: false,
        navigationHelpButton: false,
        creditContainer: document.createElement('div')
      });

      // Add OSM Buildings
      try {
        const osmBuildings = await Cesium.createOsmBuildingsAsync();
        viewer.scene.primitives.add(osmBuildings);
      } catch (e) {
        console.warn('Could not load OSM Buildings:', e);
      }

      // Set initial camera position (New York City as default)
      viewer.camera.setView({
        destination: Cesium.Cartesian3.fromDegrees(-74.006, 40.7128, 1500),
        orientation: {
          heading: Cesium.Math.toRadians(0),
          pitch: Cesium.Math.toRadians(-45),
          roll: 0
        }
      });

      // Notify parent that viewer is ready
      if (onReady) {
        onReady(viewer);
      }
    } catch (e) {
      console.error('Failed to initialize Cesium:', e);
      error = e instanceof Error ? e.message : 'Failed to load 3D map';
    }
  });

  onDestroy(() => {
    if (viewer && !viewer.isDestroyed()) {
      viewer.destroy();
    }
  });
</script>

{#if error}
  <div class="cesium-error">
    <p>Error: {error}</p>
  </div>
{:else}
  <div class="cesium-container" bind:this={container}></div>
{/if}

<style>
  .cesium-container {
    width: 100%;
    height: 100%;
  }

  .cesium-error {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #1a1a2e;
    color: #f87171;
    padding: 2rem;
    text-align: center;
  }

  :global(.cesium-viewer) {
    width: 100%;
    height: 100%;
  }

  :global(.cesium-viewer-bottom) {
    display: none !important;
  }
</style>
