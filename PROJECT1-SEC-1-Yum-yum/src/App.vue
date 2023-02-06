<script setup>
import { ref } from 'vue'

//variable
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
      deck.push(point + "-" + type); //A-C -> K-C, A-D -> K-D
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
  <div class="w-full py-12">
    <!-- Dealer -->
    <div class="w-full">
      <div class="flex justify-center space-x-8">
        <img class="w-36" src='./assets/img/All-card-final/backcard/back_card.svg' ref="hiddenCard">
        <img class="w-36" src='./assets/img/All-card-final/7-C.png'>
      </div>
      <h1 class="pt-4 flex justify-center text-white font-bold text-xl">Dealer: {{ dealerPoint }}</h1>
    </div>

    <!-- Button -->
    <div class="flex justify-center w-full py-3 space-x-8">
      <button class="px-6 items-center bg-green-500 hover:bg-green-600 active:bg-green-800 text-white 
               font-bold text-lg text-center rounded-lg py-2"> HIT
      </button>
      <button class="px-6 items-center bg-red-500 hover:bg-red-600 active:bg-red-800 text-white 
               font-bold text-lg text-center rounded-lg py-2"> STAY
      </button>
    </div>

    <!-- Player -->
    <div class="w-full">
      <h1 class="flex justify-center text-white font-bold text-xl">Player: {{ playerPoint }}</h1>
      <div class="flex justify-center space-x-5 pt-4">
        <img class="w-36 " src="./assets/img/All-card-final/J-T.png">
        <img class="w-36 " src="./assets/img/All-card-final/A-C.png">
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
