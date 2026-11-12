<script setup>
// npm/Vite version: import from the 'vue' package instead of reading the global `Vue`
// that the CDN <script> tag created. (createApp now lives in main.js.)
import { ref, computed } from 'vue'

const scrabbleValues = {
  A: 1, B: 3, C: 3, D: 2,
  E: 1, F: 4, G: 2, H: 4,
  I: 1, J: 8, K: 5, L: 1,
  M: 3, N: 1, O: 1, P: 3,
  Q: 10, R: 1, S: 1, T: 1,
  U: 1, V: 4, W: 4, X: 8,
  Y: 4, Z: 10,
};

// STATE: a ref is a reactive "box". In JS you use .value; in the template you don't.
const word = ref('');
const doubleWord = ref(false);

// DERIVED STATE: computed() re-runs automatically when word.value changes
const letters = computed(() =>
  Array.from(word.value.toUpperCase().replace(/[^A-Z]/g, ''))
);

const score = computed(() => {
  let sum = 0;
  for (const l of letters.value) {   // `letters` is a ref → loop over letters.value
    sum += scrabbleValues[l];
  }
  if (doubleWord.value) {
    sum = sum * 2;
  }
  return sum;
  // can also use .reduce:
  // return letters.value.reduce((sum, l) => sum + scrabbleValues[l], 0)
});

// No `return { ... }` needed: with <script setup>, every top-level
// variable is automatically available in the template.

// Notice what's missing: no querySelector, no addEventListener,
// no innerHTML, no "hidden = true". We describe the UI; Vue keeps it in sync.
</script>

<template>
  <main>
    <h1>Scrabble Score <small>(Vue with npm)</small></h1>

    <label for="word">Word</label>
    <!-- v-model: two-way binding between the input and the `word` ref -->
    <input id="word" v-model="word" type="text" autocomplete="off" placeholder="Type a word">

      <label for="double-word"><input id="double-word" type="checkbox" v-model="doubleWord"> Double word</label>

    <!-- v-if / v-else: Vue adds/removes these for us -->
    <p v-if="letters.length === 0" class="hint">Enter a word to see its score.</p>

    <template v-else>
      <div class="tiles">
        <!-- v-for: one tile per letter. :class binds an attribute to an expression -->
        <span
          v-for="(l, i) in letters"
          :key="i"
          class="tile"
          :class="{ high: scrabbleValues[l] >= 8 }"
        >{{ l }}<sub>{{ scrabbleValues[l] }}</sub></span>
      </div>
      <!-- {{ }}: output a value. `score` is a computed, so it is always current -->
      <p class="score">Score: {{ score }}</p>
    </template>

    <!-- TRY IT: add a "double word score" checkbox.
         1. const doubleWord = ref(false)
         2. <input type="checkbox" v-model="doubleWord">
         3. update the score computed: return doubleWord.value ? total * 2 : total
    -->
  </main>
</template>

<style>
body {
  font-family: system-ui, sans-serif;
  background: #f6f3ea;
  color: #222;
  margin: 0;
}
main {
  max-width: 36rem;
  margin: 3rem auto;
  padding: 0 1rem;
}
h1 small { font-weight: normal; color: #777; font-size: 0.6em; }
label { display: block; font-weight: 600; font-size: 1.25rem;  margin-bottom: 0.25rem; }
input[type="text"] {
  font-size: 1.5rem;
  padding: 0.5rem;
  width: 100%;
  box-sizing: border-box;
}
.hint { color: #777; }
label[for=double-word] { margin-top: 0.5rem; font-weight: normal; }
input[type=checkbox] { width: 1.15rem; height: 1.15rem; }
.tiles { display: flex; flex-wrap: wrap; gap: 0.4rem; margin: 1.25rem 0; }
.tile {
  position: relative;
  width: 2.75rem;
  height: 2.75rem;
  display: grid;
  place-items: center;
  background: #f1d9a7;
  border: 1px solid #c9a96a;
  border-radius: 4px;
  font-size: 1.5rem;
  font-weight: 700;
  box-shadow: 0 2px 0 #b8955a;
}
.tile sub { position: absolute; right: 4px; bottom: 2px; font-size: 0.65rem; }
.tile.high { background-color: #f3b7a0; }
.score { font-size: 2rem; font-weight: 700; }


</style>
