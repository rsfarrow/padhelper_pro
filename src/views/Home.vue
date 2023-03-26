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
const selectedNum = ref(0);
const combo = reactive([]);
let longRange = {
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

let midRange = {
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

let shortRange = {
  next: ["shortRange", "midRange"],
  strikes: ["elbow", "knee"],
  // strikes: ["left elbow", "right elbow", "left knee", "right knee"],
};
function getNextStrike({ strikes, next }) {
  const strikesLength = strikes.length;
  const rangeLength = next.length;
  return {
    strike: strikes[this.getRandomInt(strikesLength)],
    next: next[this.getRandomInt(rangeLength)],
  };
}
function selectOption(choice) {
  console.log('this.combo', this.combo);
  if (this.combo?.length) this.combo?.splice(0, this.combo.length);
  this.selectedNum = choice;
  this.generateCombo();
}
function generateCombo() {
  let nextUp = [this.longRange, this.midRange, this.shortRange][
    this.getRandomInt(3)
  ];
  for (let i = 0; i < this.selectedNum; i++) {
    let { strike, next } = this.getNextStrike(nextUp);
    this.combo.push(strike);
    nextUp = this[next];
  }
}
function getRandomInt(max) {
  return Math.floor(Math.random() * max);
}
</script>

