<script setup>
import CardSquare from './components/CardSquare.vue';
import { ref, computed, onMounted } from 'vue';

onMounted(() => {
  loadCard();
  console.log('mounted!');
})

const cardPool = [
    "Money",
    "Flip Flop",
    "Hunch",
    "Pastrami",
    "Out of Bounds",
    "Off the Hook",
    "Worchester-shire",
    "Distract and Sneak Food",
    "Tighten it Up",
  ];

const cardSize = 3;
const card = ref([]);
const showWinner = ref(true);
const isLoading = ref(false);
const isShowHelp = ref(false);

const isComplete = computed(() => {
  let unstampedSquares = card.value.find(o => o.Stamped === false);
  return !unstampedSquares ? true : false;
})

const stampSquare = (index) => {
  card.value[index].Stamped = true;
};

const unstampSquare = (index) => {
  card.value[index].Stamped = false;
  showWinner.value = true;
};

function showHelp() {
  isShowHelp.value = true;
}

function hideHelp() {
  isShowHelp.value = false;
}

function hideWinner() {
  showWinner.value = false;
}

function loadCard() {
  showWinner.value = true;
  isShowHelp.value = false;
  isLoading.value = true;
  clearCard();
  var size = cardSize * cardSize;
  getRandomSubarray(cardPool, size).forEach(element => {
    card.value.push({
      Text: element,
      Stamped: false,
    })
  });

  setTimeout(function(){
    isLoading.value = false;
  }, 2000);
}

function clearCard() {
  card.value = [];
}

function getRandomSubarray(arr, size) {
  var shuffled = arr.slice(0), i = arr.length, min = i - size, temp, index;
  while (i-- > min) {
    index = Math.floor((i + 1) * Math.random());
    temp = shuffled[index];
    shuffled[index] = shuffled[i];
    shuffled[i] = temp;
  }

  return shuffled.slice(min);
}

</script>

<template>
  <main class="container">
    <div class="title-container">
      <div>
        <img class="title-image" src="./assets/flavortown_bingo.png">
      </div>
      <nav>
        <a @click="showHelp()">Help</a> | <a @click="loadCard()">Load New Card</a>
      </nav>
    </div>
    <div class="card-container">
      <div class="card">
        <CardSquare v-for="(square, index) in card" :key="index" :text="square.Text" :stamped="square.Stamped" :index="index" @stampSquare="stampSquare" @unstampSquare="unstampSquare" />
      </div>
      <div class="card-winner-wrapper" v-if="isComplete && showWinner">
        <div class="card-winner">
          <div class="card-winner-title">OUT OF BOUNDS</div>
          <div class="button"><button @click="hideWinner()">Muchas Gracias</button></div>
        </div>
      </div>
      <div class="card-loading" v-if="isLoading">
        <div class="card-loading-title">Rollin' Out</div>
        <div class="roll-out"><img src="../camaro.png"></div>
      </div>
      <div class="help" v-if="isShowHelp">
        <h2>My brother from another mother,</h2>
        <p>During your next Flavortown Roadtrip, each player loads a Bingo card and stamps it each time the item occurs on the show.</p>
        <p>The first player to stamp all items wins.</p>
        <p>Currently, only blackout is supported. In the future, other methods of winning will be added.</p>
        <div class="button"><button @click="hideHelp()">Muchas Gracias</button></div>
      </div>
    </div>
  </main>
</template>

<style scoped>
nav {
  width: 100%;
  padding: 10px;
  text-align: center;
  font-size: medium;
}

.container {
  display: block;
  padding: 10px;
}

.container > * { flex: 1; }

.title-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  background: black;
}

.title-image {
  max-width: 90%;
  max-height: 90%;
}

.card-container {
  display: grid;
  grid-template: 1fr / 1fr;
  place-items: center;
}

.card-container > * {
  grid-column: 1 / 1;
  grid-row: 1 / 1;
  height: 100%;
  width: 100%;
}

.card {
  z-index: 1;
  display: grid;
  gap: 1px;
  grid-template-columns: repeat(3, 1fr);
  background: rgb(255, 149, 0);
}

.card-loading {
  z-index: 3;
  position: relative;
  display: flex;
  overflow: hidden;
  background-color: rgba(177, 43, 22, 0.9);
}

.card-loading > .card-loading-title {
  width: 100%;
  margin-top: 25%;
  align-self: flex-start;
  text-align: center;
  font-weight: bold;
  font-size: 5em;
}

.card-loading > .roll-out {
  align-self: flex-end;
  position: absolute;
  -webkit-animation:rollOut 2s normal forwards;
  -o-animation:rollOut 2s normal forwards;
  -moz-animation:rollOut 2s normal forwards;
  animation:rollOut 2s normal forwards;
}

@keyframes rollOut {
  0% {
    left: 0%;
  }
  100% {
    left: 100%;
  }
}

.card-winner-wrapper {
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  width: 100%;
  padding: 10px;
  background-color: rgba(0, 0, 0, 0.5);
}

.card-winner {
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: center;
  height: 50%;
  width: 50%;
  padding: 10px;
  background-color: orange;
  color: black;
  text-align: center;
}

.card-winner > .card-winner-title {
  font-size: xx-large;
  font-weight: bold;
}

.card-winner > .button {
  display: flex;
  justify-content: center;
  padding-top: 10px;
}
.card-winner > * {
  font-weight: bold;
}

.help {
  z-index: 4;
  height: 100%;
  width: 100%;
  padding: 10px;
  background-color: orange;
  color: black;
  text-align: justify;
}

.help > * {
  font-weight: bold;
}

.help > .button {
  display: flex;
  justify-content: center;
  padding-top: 10px;
}

.help > button {
  justify-self: center;
}

@media only screen and (min-width: 600px) {
  .container {
    display: flex;
    width: 100%;
  }
}

</style>
