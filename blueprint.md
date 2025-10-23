# Project Blueprint

## Overview

This project is a Svelte-based application that creates a full-screen, interactive grid of cells. Users can click on any cell to split it into two smaller cells. The application is designed to be visually engaging and demonstrate the power of Svelte's reactivity and component-based architecture.

## Implemented Features

*   **Core Functionality:**
    *   A full-screen grid that starts with a single cell.
    *   Each cell has a unique ID to ensure stability when splitting.
    *   Clicking a cell splits it into two new cells (either horizontally or vertically).
        *   One of the new cells retains the parent cell's hue, while the other receives a new hue rotation.
    *   A limit is set on cell division to prevent performance issues.
    *   Click events use `stopPropagation` to prevent unintended side effects.
    *   A reset button allows the user to start over.
    *   A slider control to adjust the animation speed of the background and cells.
    *   An automatic cell splitting feature that can be toggled on and off.
    *   A slider to control the rate of automatic cell splitting.
    *   Controls automatically hide after 5 seconds of inactivity and reappear on mouse movement.
*   **Component Structure:**
    *   `App.svelte`: The main application component that renders the grid and controls, and manages control visibility.
    *   `Grid.svelte`: A component that manages the state of the grid and its cells.
    *   `Cell.svelte`: A component that represents a single cell in the grid.
    *   `Controls.svelte`: A component that contains the animation speed slider, auto-split slider, and auto-split checkbox.
*   **State Management:**
    *   Svelte stores are used to share state between components:
        *   `animationSpeed`: Controls the speed of the background and cell animations.
        *   `autoSplitRate`: Controls the rate at which cells are automatically split.
        *   `autoSplitEnabled`: Toggles the automatic splitting feature.
        *   `controlsVisible`: Toggles the visibility of the UI controls.
*   **Styling:**
    *   The application runs in full-screen mode.
    *   The grid has a radial gradient background that spans the entire hue range.
    *   The background gradient continuously cycles through the entire hue range.
    *   Each cell uses a `backdrop-filter` to apply a `hue-rotate` to the background gradient, with the rotation amount increasing with each cell division.
    *   Each cell's color continuously cycles through the hue range.
    *   Cells have a subtle, semi-transparent white border and a smooth transition effect on property changes.
    *   The cursor changes to a pointer when hovering over a cell.
    *   A "glassmorphism" style is applied to the reset button and controls.
    *   Controls animate off-screen when hidden.

## Current Plan

### Auto-Hide Controls on Inactivity

*   [x] Add a `controlsVisible` boolean to the Svelte store.
*   [x] In `App.svelte`, add a `mousemove` event listener to show the controls and set a 5-second timer to hide them.
*   [x] In `Controls.svelte`, use the `controlsVisible` store value to add a class that animates the controls off-screen (downwards).
*   [x] In `Grid.svelte`, use the `controlsVisible` store value to add a class that animates the reset button off-screen (upwards).
