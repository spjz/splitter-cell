
<script lang="ts">
  import { onMount } from 'svelte';
  import { animationSpeed } from './store';

  export let divisions: number;

  let hue = divisions * 30;
  let element: HTMLDivElement;
  let speed = 1;

  animationSpeed.subscribe(value => {
    speed = value;
  });

  onMount(() => {
    function animate() {
      hue = (hue + 0.5 * speed) % 360;
      element.style.setProperty('--hue-rotation', `${hue}deg`);
      requestAnimationFrame(animate);
    }

    animate();
  });
</script>

<div bind:this={element} class="cell" style="--hue-rotation: {hue}deg;">
</div>

<style>
  .cell {
    width: 100%;
    height: 100%;
    background-color: transparent;
    backdrop-filter: hue-rotate(var(--hue-rotation));
    -webkit-backdrop-filter: hue-rotate(var(--hue-rotation));
    transition: backdrop-filter 0.3s ease;
  }
</style>
