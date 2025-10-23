
<script lang="ts">
  import { animationSpeed, autoSplitRate, autoSplitEnabled, controlsVisible } from './store';
</script>

<div class="controls-container" class:hidden={!$controlsVisible}>
  <div class="control">
    <label for="speed-slider">Animation Speed</label>
    <input
      id="speed-slider"
      type="range"
      min="0.1"
      max="5"
      step="0.1"
      bind:value={$animationSpeed}
    />
  </div>
  <div class="control">
    <label for="autosplit-slider">Auto-Split Rate</label>
    <input
      id="autosplit-slider"
      type="range"
      min="0.1"
      max="10"
      step="0.1"
      bind:value={$autoSplitRate}
    />
  </div>
  <div class="control checkbox-control">
    <label for="autosplit-checkbox">Auto-Split</label>
    <input
      id="autosplit-checkbox"
      type="checkbox"
      bind:checked={$autoSplitEnabled}
    />
  </div>
</div>

<style>
  .controls-container {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 10;
    padding: 15px 25px;
    background-color: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 15px;
    color: white;
    font-family: sans-serif;
    font-size: 16px;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    display: flex;
    align-items: center;
    gap: 25px;
    transition: transform 0.5s ease-in-out;
  }

  .controls-container.hidden {
      transform: translateX(-50%) translateY(calc(100% + 30px));
  }

  .control {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .checkbox-control {
    gap: 15px;
  }

  input[type="range"] {
    -webkit-appearance: none;
    appearance: none;
    width: 150px;
    height: 8px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 5px;
    outline: none;
    opacity: 0.7;
    transition: opacity .2s;
  }

  input[type="range"]:hover {
    opacity: 1;
  }

  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 20px;
    height: 20px;
    background: white;
    border-radius: 50%;
    cursor: pointer;
  }

  input[type="range"]::-moz-range-thumb {
    width: 20px;
    height: 20px;
    background: white;
    border-radius: 50%;
    cursor: pointer;
  }

  input[type="checkbox"] {
    width: 20px;
    height: 20px;
    cursor: pointer;
  }
</style>
