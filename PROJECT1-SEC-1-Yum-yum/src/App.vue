<script setup>
import { ref } from 'vue'

//variable
let bg_first = ref(true)
const bg_first_func = () => {
  bg_first.value = false
}

let how_to_play = ref(false)
const how_to_play_func = () => {
  how_to_play.value = !how_to_play.value
}

//variable game
let deck = [];
let dealerPoint = ref('17')
let playerPoint = ref('20')

//card
let dealerCard01 = 'img/All-card-final/backcard/back_card.svg'
let dealerCard02 = 'img/All-card-final/' + '9' + '-' + 'C' + '.png'
let playerCard01 = 'img/All-card-final/' + '9' + '-' + 'T' + '.png'
let playerCard02 = 'img/All-card-final/' + 'A' + '-' + 'N' + '.png'

//Call Functions
buildDeck();
shuffleDeck();
// startGame();

//Deck
function buildDeck() {
  let points = ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"];
  let types = ["A", "C", "N", "T"];
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

// function startGame() {
//   let cardDealer = deck.shift(); //first Card
//   // console.log(cardDealer);
//   // console.log(deck);
// }
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
      <div class="flex justify-center items-center" v-show="!bg_first">
        <div class="relative flex justify-center">
          <div class="totalScore z-10 relative flex items-center justify-center "><span class="pt-10 "> Round: </span>
          </div>
          <div class="absolute top-0 m-auto z-30 round">
            <span class="flex justify-center items-center w-60 h-12 font-lg font-bold">1 - 0</span>
          </div>
          <div class="player absolute m-auto z-20 flex items-center top-0 bx6  justify-around">
            <div class="flex">PLAYER:</div>
            <div class="flex"></div>
            <div class="flex">DEALER:</div>
          </div>
        </div>
      </div>

      <!-- Btn How to Play -->
      <div class="w-full absolute" v-show="!bg_first">
        <button class="px-3 py-1 mr-12 float-right
         bg-yellow-600 hover:bg-yellow-700 active:bg-yellow-800 text-black 
               font-bold text-lg text-center rounded-lg" @click="how_to_play_func">
          How to Play
        </button>
      </div>

      <!-- Text How to play -->
      <div class="rule absolute w-2/5 h-4/5 bg-white opacity-90" v-show="how_to_play">
        <p class="pt-6 text-center text-3xl">อธิบายการเล่น</p>
      </div>

      <!-- Dealer -->
      <div class="w-full pt-6">
        <div class="flex justify-center space-x-8">
          <img class="w-32" :src="dealerCard01" ref="hiddenCard">
          <img class="w-32" :src="dealerCard02">
        </div>
        <h1 class="flex justify-center text-white font-bold text-xl py-4">Dealer: {{ dealerPoint }}</h1>
      </div>

      <!-- Button -->
      <div class="w-full flex justify-center space-x-8 h-8">
        <button class="px-6 bg-green-500 hover:bg-green-600 active:bg-green-800 text-white 
               font-bold text-lg text-center rounded-lg"> HIT
        </button>
        <button class="px-6 bg-red-500 hover:bg-red-600 active:bg-red-800 text-white 
               font-bold text-lg text-center rounded-lg"> STAY
        </button>
      </div>

      <!-- Player -->
      <div class="w-full">
        <h1 class="flex justify-center text-white font-bold text-xl py-4">Player: {{ playerPoint }}</h1>
        <div class="flex justify-center space-x-5">
          <img class="w-32" :src="playerCard01">
          <img class="w-32" :src="playerCard02">
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
