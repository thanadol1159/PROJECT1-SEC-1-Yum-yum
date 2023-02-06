<script setup>
import { ref } from 'vue'

//variable
let bg_first = ref(true)
const bg_first_func = () => {
  bg_first.value = false
  how_to_play.value = true
}

let how_to_play = ref(false)
const how_to_play_func = () => {
  how_to_play.value = !how_to_play
}

//variable game
let deck = [];
let dealerPoint = ref('17')
let playerPoint = ref('20')

// let hide = ref('./assets/img/All-card-final/backcard/back_card.svg')


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

    <div class="flex justify-center items-center">
      <div class="relative flex justify-center">
        <div class="box5 z-10 relative flex items-center justify-center bx5"><span class="pt-10 "> Round: </span></div>
        <div class="box7 absolute top-0 m-auto z-30 bx7"><span class="flex justify-center items-center w-60 h-12">1 - 0</span></div>
        <div class="box6 absolute m-auto z-20 flex items-center top-0 bx6  justify-around">
          <div class="flex ">PLAYER:</div>
          <div class="flex">2</div>
          <div class="flex">DEALER:</div>
        </div>
      </div>
    </div>

  <div class="absolute w-screen h-screen bg-bottom bg-cover bg-no-repeat bg-[url('../src/assets/img/bg-blur.jpg')]"
    @click="bg_first_func" v-show="bg_first">
    <h1 class="w-full flex justify-center mt-9 font-sans font-extrabold text-9xl tracking-wider text-white">BLACKJACK
    </h1>
    <h4 class="w-full font-sans font-semibold text-3xl flex justify-center text-end mt-96 text-gray-300">click to start
    </h4>
  </div>

  <div class="w-screen h-screen">
    <div class="bg-bottom bg-cover bg-no-repeat w-full h-full bg-[url('./src/assets/img/bg-fix-fix.png')]">
      <!-- Btn How to Play -->
      <!-- <div class="w-full absolute">
        <button class="px-3 py-1 mt-14 mr-28 float-right
         bg-yellow-600 hover:bg-yellow-700 active:bg-yellow-800 text-white 
               font-bold text-lg text-center rounded-md">
          How to Play
        </button>
      </div> -->

      <!-- Text How to play -->
      <!-- <div class="rule absolute w-2/5 h-4/5 bg-white opacity-90">
        <p class="pt-6 text-center text-3xl">อธิบายการเล่น</p>
      </div> -->

      <!-- Dealer -->
      <div class="w-full pt-14">
        <div class="flex justify-center space-x-8">
          <img class="w-36" src='./assets/img/All-card-final/backcard/back_card.svg' ref="hiddenCard">
          <img class="w-36" src='./assets/img/All-card-final/7-C.png'>
        </div>
        <h1 class="flex justify-center text-white font-bold text-xl py-4">Dealer: {{ dealerPoint }}</h1>
      </div>

      <!-- Button -->
      <div class="w-full flex justify-center space-x-8 h-12">
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
          <img class="w-36" src="./assets/img/All-card-final/J-T.png">
          <img class="w-36" src="./assets/img/All-card-final/A-C.png">
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.rule {
  left: 30%;
  top: 10%;
}
  .box5{
    width: 220px;
    height: 75px;
  }
  .box6{
    width: 600px;
    height: 40px;
  }

  .bx5{
    background: linear-gradient(120deg,#00e8fd,#027fbe);
    border-bottom-left-radius: 50px;
    border-bottom-right-radius: 50px;
  }
  .bx6{
    background: linear-gradient(120deg,#19f0b0,#0fee98);
    border-bottom-left-radius: 25px;
    border-bottom-right-radius: 25px;
  }
  .bx7{
    background: linear-gradient(120deg,#015b40,#023f28);
    border-bottom-left-radius: 25px;
    border-bottom-right-radius: 25px;
  }
</style>
