# Scrabble Score — Vue with npm

A small example app that calculates the Scrabble score of a word as you type.
It's the **npm + Vite** version of a Vue app we first built by loading Vue from a
CDN `<script>` tag. The app does the same thing; what changes is how the project
is organized and run.

## What to look at

| File | What it does |
| --- | --- |
| [`index.html`](index.html) | The single HTML page. It has an empty `<div id="app">` and loads `src/main.js`. |
| [`src/main.js`](src/main.js) | The entry point. It imports `createApp` from the `vue` package and mounts `App` into `#app`. |
| [`src/App.vue`](src/App.vue) | A **Single-File Component (SFC)**. The `<script setup>`, `<template>`, and `<style>` all live in one file. **Most of the code you'll read is here.** |
| [`package.json`](package.json) | Lists dependencies (`vue`, `vite`) and the `npm run` scripts. |
| [`vite.config.js`](vite.config.js) | Configures Vite, the dev server and build tool. |

### Vue concepts used in `App.vue`

- **`ref()`**: reactive state (`word`, `doubleWord`). In JavaScript you use `.value`; in the template you don't.
- **`computed()`**: derived values (`letters`, `score`) that recalculate automatically when the state they depend on changes.
- **`v-model`**: two-way binding between form inputs and state.
- **`v-if` / `v-else`**, **`v-for`**, **`:class`**, and **`{{ }}`**: template directives for showing things conditionally, repeating them for each item, setting classes, and printing values.

Notice there's no `querySelector`, `addEventListener`, or `innerHTML`. You
describe what the UI should look like for a given state, and Vue keeps the page
in sync with that state.

### CDN version vs. npm version

- **CDN:** Vue arrives as a global `Vue` object from a `<script>` tag, and everything sits in one HTML file.
- **npm:** Vue is installed into `node_modules/` and brought in with `import { ref, computed } from 'vue'`. Vite turns `.vue` files into code the browser can run.

## Running the app

**Prerequisite:** [Node.js](https://nodejs.org/) (version 22.18+ or 24.12+). Run `node -v` to see which version you have.

1. Install dependencies. You only need to do this once, or again after `package.json` changes:

   ```sh
   npm install
   ```

2. Start the development server:

   ```sh
   npm run dev
   ```

3. Open the URL shown in the terminal (usually <http://localhost:5173>).

4. Edit `src/App.vue` and save. The browser updates right away; you don't need to reload (this is called *hot module replacement*).

5. Press `Ctrl+C` in the terminal to stop the server.

### Other scripts

- `npm run build`: builds an optimized production version into `dist/`.
- `npm run preview`: serves the `dist/` build locally so you can check it.

## Try it

`App.vue` has a **Double word** checkbox. To practice, try adding another
feature, such as a **Triple word** option or a letter-count display.

## Recommended tools

- VS Code with the [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) extension
- Vue.js devtools browser extension for [Chrome](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) or [Firefox](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
