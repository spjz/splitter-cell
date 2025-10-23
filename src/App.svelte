<script lang="ts">
  import Grid from './lib/Grid.svelte';
  import Controls from './lib/Controls.svelte';
  import { onMount, onDestroy } from 'svelte';
  import { controlsVisible } from './lib/store';

  let inactivityTimer: any;

  function handleMouseMove() {
    controlsVisible.set(true);
    clearTimeout(inactivityTimer);
    inactivityTimer = setTimeout(() => {
      controlsVisible.set(false);
    }, 2500);
  }

  onMount(() => {
    window.addEventListener('mousemove', handleMouseMove);
    // Initially hide controls after a delay
    inactivityTimer = setTimeout(() => {
      controlsVisible.set(false);
    }, 2500);
  });

  onDestroy(() => {
    window.removeEventListener('mousemove', handleMouseMove);
    clearTimeout(inactivityTimer);
  });
</script>

<main>
  <Grid />
  <Controls />
</main>

<style>
  main {
    padding: 0;
    margin: 0;
  }
</style>
