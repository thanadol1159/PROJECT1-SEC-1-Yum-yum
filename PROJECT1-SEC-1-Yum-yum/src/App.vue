<script setup>
import { ref } from 'vue'
//variable
let bg_first = ref(true);
const bg_first_func = () => {
  bg_first.value = false;
}
let showHit = ref(true)
//variable game
let deck = [], dealerArr = [], playerArr = [];
let dealerPoint = ref(0);
let playerPoint = ref(0);
let playerScorePoint = ref(0);
let dealerScorePoint = ref(0);
let numRound = ref(1);
let textWLB = ref();
// let testA = ["2-S", "3-C", "A-C", "7-D", "2-H", "5-S", "9-S", "2-S"]
let firstCard = [];

//Call Functions
window.onload = function () {
  buildDeck();
  shuffleDeck();
  startGame();
}

//Deck
function buildDeck() {
  let points = ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"];
  // let types = ["A", "C", "N", "T"];
  let types = ["C", "D", "H", "S"];
  for (const type of types) {
    for (const point of points) {
      deck.push(point + "-" + type);  // A-A , 2-A , ... , K-T
    }
  }
  console.log(deck);
}
function shuffleDeck() {
  for (let i = 0; i < deck.length; i++) {
    let j = Math.floor(Math.random() * deck.length); // (0-1) * 52 => (0-51.9999)
    let temp = deck[i];
    deck[i] = deck[j];
    deck[j] = temp;
  }
  console.log(deck);
}

function startGame() {
  // FirstCard
  let card = deck.shift(); // J-T
  firstCard.push(getPicture(card));
  dealerPoint.value += getPoint(card, dealerPoint);
  // Dealer
  let cardDealer = deck.shift();
  dealerArr.push(getPicture(cardDealer));
  dealerPoint.value += getPoint(cardDealer, dealerPoint);
  for (let i = 0; i < 2; i++) {
    // Player
    let cardPlayer = deck.shift();
    playerArr.push(getPicture(cardPlayer));
    playerPoint.value += getPoint(cardPlayer, playerPoint);
  }
  console.log(playerPoint.value);
  console.log(dealerPoint.value);
}

function getPicture(card) {
  // let src = 'img/All-card-final/' + card + '.png';
  let src = 'testcard/cards/' + card + '.png';
  return src;
}

function getPoint(card, yourPoint) {
  let point = card.split("-")[0] // "6-T" -> ["6", "T"]
  point == 'A' ? (yourPoint.value + 11 > 21) ? point = 1 : point = 11 : isNaN(point) ? point = 10 : point
  return parseInt(point);
}

function hit() {
  let cardPlayer = deck.shift();
  playerArr.push(getPicture(cardPlayer));
  playerPoint.value += getPoint(cardPlayer, playerPoint);
  if (playerPoint.value > 21) {
    showHit.value = false;
    textWLB.value = "BUST"
    stay()
  }
}

function stay() {
  showHit.value = false;
  if (textWLB.value != "BUST" && dealerPoint.value < 17) {
    for (let i = 1; dealerPoint.value < 17; i++) {
      let cardDealer = deck.shift();
      dealerArr.push(getPicture(cardDealer));
      dealerPoint.value += getPoint(cardDealer, dealerPoint);
      if (dealerPoint.value > 21) {
        dealerPoint.value = "BUST"
        playerScorePoint.value++
      }
    }
  }
  if (textWLB.value == "BUST") {
    textWLB.value = "BUST"
    dealerScorePoint.value++
  }
  else if (dealerPoint.value > playerPoint.value) {
    textWLB.value = "Lose"
    dealerScorePoint.value++
  }
  else if (dealerPoint.value < playerPoint.value) {
    textWLB.value = "Win"
    playerScorePoint.value++
  }
  else if (dealerPoint.value == playerPoint.value) {
    textWLB.value = "Tie"
  }
}
</script>

<template>
  <div class="w-screen h-screen">
    <!-- BG Fix -->
    <div class="bg-bottom bg-cover bg-no-repeat w-full h-full bg-[url('img/bg-fix-fix.png')]">
      <!-- BG Blur -->
      <div class="absolute w-screen h-screen bg-bottom bg-cover bg-no-repeat bg-[url('img/bg-blur.jpg')]"
        @click="bg_first_func" v-show="bg_first">
        <h1 class="w-full flex justify-center mt-9 
        font-sans font-extrabold text-9xl tracking-wider text-white">
          BLACKJACK
        </h1>
        <h4 class="w-full font-sans font-semibold text-3xl flex justify-center 
        text-end mt-64 text-gray-300">
          click to start
        </h4>
      </div>

      <!-- Score -->
      <div class="flex justify-center items-center text-stone-900" v-show="!bg_first">
        <div class="relative flex justify-center">
          <div class="totalScore z-10 relative flex items-center justify-center "><span class="pt-10 "> Round: {{
            numRound
          }} </span>
          </div>
          <div class="absolute top-0 m-auto z-30 round">
            <span class="compare flex justify-center items-center w-60  text-2xl font-bold">
              <span class="text-teal-300 mr-4">{{ playerScorePoint }}</span> - <span class="text-rose-600 ml-4">{{
                dealerScorePoint
              }} </span>
            </span>
          </div>
          <div class="player absolute m-auto z-20 flex items-center top-0 bx6  justify-around">
            <div class="flex">PLAYER: {{ playerPoint }}</div>
            <div class="flex w-16"></div>
            <div class="flex">DEALER: <span v-show="!showHit">{{ dealerPoint }}</span></div>
          </div>
        </div>
      </div>

      <!-- Btn How to Play -->
      <!-- <div class="w-full absolute" v-show="!bg_first">
        <button class="px-3 py-1 mr-12 float-right
         bg-yellow-600 hover:bg-yellow-700 active:bg-yellow-800 text-black 
               font-bold text-lg text-center rounded-lg" @click="how_to_play_func">
          How to Play
        </button>
      </div> -->

      <!-- Text How to play -->
      <!-- <div class="rule absolute w-2/5 h-4/5 bg-white opacity-90" v-show="how_to_play">
        <p class="pt-6 text-center text-3xl">อธิบายการเล่น</p>
      </div> -->

      <!-- Dealer -->
      <div class="w-full pt-6">
        <div class="flex justify-center space-x-5" ref="imgCardDealer">
          <img v-if="showHit" src="img/All-card-final/backcard/back_card.svg" class="w-32">
          <img v-else :src="firstCard" class="w-32">
          <img v-for="card in dealerArr" :src=card class="w-32">
          <!-- popup -->
          <div v-show="!showHit">
            {{ textWLB }}
          </div>
        </div>
      </div>
      <h1 class="flex justify-center text-white font-bold text-xl py-4">
      </h1>

      <!-- Button -->
      <div class="w-full flex justify-center space-x-8 h-8">
        <button type="button" class="px-6 bg-green-500 hover:bg-green-600 active:bg-green-800 text-white 
               font-bold text-lg text-center rounded-lg" @click="hit" :disabled="!showHit"
          :class="showHit ? 'none' : 'bg-gray-500 hover:bg-gray-500 active:bg-gray-500 cursor-not-allowed'">
          HIT
        </button>
        <button type="button" class="px-6 bg-red-500 hover:bg-red-600 active:bg-red-800 text-white 
               font-bold text-lg text-center rounded-lg" @click="stay" :disabled="!showHit"
          :class="showHit ? 'none' : 'bg-gray-500 hover:bg-gray-500 active:bg-gray-500 cursor-not-allowed'">
          STAY
        </button>
      </div>

      <!-- Player -->
      <div class="w-full">
        <h1 class="flex justify-center text-white font-bold text-xl py-4">
        </h1>
        <div class="flex justify-center space-x-5" ref="imgCardPlayer">
          <img v-for="card in playerArr" :src=card class="w-32">
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.rule {
  left: 30%;
  top: 14%;
}

.compare {
  height: 40px;
}

.totalScore {
  width: 220px;
  height: 75px;
  background: linear-gradient(120deg, #00e8fd, #027fbe);
  border-bottom-left-radius: 50px;
  border-bottom-right-radius: 50px;
}

.player {
  width: 600px;
  height: 40px;
  background: linear-gradient(120deg, #19f0b0, #0fee98);
  border-bottom-left-radius: 25px;
  border-bottom-right-radius: 25px;
}


.round {
  background: linear-gradient(120deg, #015b40, #023f28);
  border-bottom-left-radius: 25px;
  border-bottom-right-radius: 25px;
}
</style>
