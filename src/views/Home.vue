<template>
  <div>Select an option below</div>
  <v-btn
    v-for="option in options"
    :key="option"
    class="d-flex my-4 mx-auto"
    @click="selectOption(option)"
  >
    {{ option }} Piece
  </v-btn>
  <div>{{ combo }}</div>
</template>

<script setup>
import { ref, reactive } from "vue";
const options = [3, 4, 5, 6, 8];
let selectedNum = ref(0);
let combo = reactive([]);
const longRange = {
  next: ["shortRange", "midRange", "longRange"],
  strikes: ["jab", "jab", "cross", "cross", "teep", "kick"],
  // strikes: [
  //   "jab",
  //   "cross",
  //   "left teep",
  //   "right teep",
  //   "left kick",
  //   "right kick",
  // ],
};

const midRange = {
  next: ["shortRange", "midRange", "longRange"],
  strikes: ["hook", "uppercut", "step knee"],
  // strikes: [
  //   "left hook",
  //   "right hook",
  //   "left uppercut",
  //   "right uppercut",
  //   "left step knee",
  //   "right step knee",
  // ],
};

const shortRange = {
  next: ["shortRange", "midRange"],
  strikes: ["elbow", "knee"],
  // strikes: ["left elbow", "right elbow", "left knee", "right knee"],
};
const rangeOptions = {shortRange, midRange, longRange};
function getNextStrike({ strikes, next }) {
  const strikesLength = strikes.length;
  const rangeLength = next.length;
  return {
    strike: strikes[getRandomInt(strikesLength)],
    next: next[getRandomInt(rangeLength)],
  };
}
function selectOption(choice) {
  console.log('combo', combo);
  if (combo?.length) combo?.splice(0, combo.length);
  selectedNum = choice;
  generateCombo();
}
function generateCombo() {
  let nextUp = [longRange, midRange, shortRange][
    getRandomInt(3)
  ];
  for (let i = 0; i < selectedNum; i++) {
    let { strike, next } = getNextStrike(nextUp);
    combo.push(strike);
    nextUp = rangeOptions[next];
  }
}
function getRandomInt(max) {
  return Math.floor(Math.random() * max);
}
</script>

