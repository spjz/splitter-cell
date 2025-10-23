
<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import Cell from './Cell.svelte';
  import { animationSpeed, autoSplitRate, autoSplitEnabled, controlsVisible } from './store';

  let nextId = 1;
  let cells = [{ id: 0, x: 0, y: 0, width: 100, height: 100, divisions: 0 }];

  function splitCell(id: number) {
    const index = cells.findIndex(c => c.id === id);
    if (index === -1) return;

    const cell = cells[index];
    const { x, y, width, height, divisions } = cell;

    if (width < 1 || height < 1 || divisions > 4) {
      return;
    }

    let newCells;
    if (width > height) {
      // Split vertically
      newCells = [
        { id: cell.id, x, y, width: width / 2, height, divisions },
        { id: nextId++, x: x + width / 2, y, width: width / 2, height, divisions: divisions + 1 },
      ];
    } else {
      // Split horizontally
      newCells = [
        { id: cell.id, x, y, width, height: height / 2, divisions },
        { id: nextId++, x, y: y + height / 2, width, height: height / 2, divisions: divisions + 1 },
      ];
    }

    const updatedCells = [...cells];
    updatedCells.splice(index, 1, ...newCells);
    cells = updatedCells;
  }

  function resetGrid() {
    cells = [{ id: 0, x: 0, y: 0, width: 100, height: 100, divisions: 0 }];
    nextId = 1;
  }

  let autoSplitInterval: any;

  $: {
    if (autoSplitInterval) {
      clearInterval(autoSplitInterval);
    }
    if ($autoSplitEnabled) {
      const rate = $autoSplitRate > 0 ? $autoSplitRate : 0.1;
      autoSplitInterval = setInterval(() => {
        const splittableCells = cells.filter(c => c.width >= 1 && c.height >= 1 && c.divisions <= 4);
        if (splittableCells.length > 0) {
            const randomIndex = Math.floor(Math.random() * splittableCells.length);
            const randomCell = splittableCells[randomIndex];
            splitCell(randomCell.id);
        }
      }, 1000 / rate);
    }
  }

  onDestroy(() => {
    if (autoSplitInterval) {
      clearInterval(autoSplitInterval);
    }
  });
</script>

<div class="grid-container" style="animation-duration: {10 / $animationSpeed}s;">
  {#each cells as cell (cell.id)}
    <div
      style="
        left: {cell.x}%; 
        top: {cell.y}%; 
        width: {cell.width}%; 
        height: {cell.height}%;
      "
      on:click|stopPropagation={() => splitCell(cell.id)}
    >
      <Cell divisions={cell.divisions} />
    </div>
  {/each}
</div>

<button class="reset-button" class:hidden={!$controlsVisible} on:click={resetGrid}>Reset</button>

<style>
  @keyframes hue-cycle {
    from {
      filter: hue-rotate(0deg);
    }
    to {
      filter: hue-rotate(360deg);
    }
  }

  .grid-container {
    position: relative;
    width: 100vw;
    height: 100vh;
    background-image: radial-gradient(circle, hsl(0, 100%, 50%), hsl(60, 100%, 50%), hsl(120, 100%, 50%), hsl(180, 100%, 50%), hsl(240, 100%, 50%), hsl(300, 100%, 50%), hsl(360, 100%, 50%));
    overflow: hidden;
    animation-name: hue-cycle;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
  }

  .grid-container > div {
    position: absolute;
    box-sizing: border-box;
    border: 1px solid rgba(255, 255, 255, 0.2);
    transition: all 0.3s ease;
    cursor: pointer;
  }

  .reset-button {
    position: fixed;
    top: 20px;
    right: 20px;
    z-index: 10;
    padding: 10px 20px;
    background-color: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 10px;
    color: white;
    font-family: sans-serif;
    font-size: 16px;
    cursor: pointer;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px); /* For Safari */
    transition: background-color 0.3s ease, transform 0.5s ease-in-out;
    transform: translateY(0);
  }

  .reset-button.hidden {
    transform: translateY(calc(-100% - 30px));
  }

  .reset-button:hover {
    background-color: rgba(255, 255, 255, 0.2);
  }
</style>
