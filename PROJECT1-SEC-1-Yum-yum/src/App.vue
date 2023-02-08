<script setup>
import { ref } from 'vue'
import setting from "./components/icons/SystemUiconsSettings.vue";
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
function openNav() {
  document.getElementById("slideBar").style.width = "384px";
  document.getElementById("slideBar").style.display = "block";
}

function closeNav() {
  document.getElementById("slideBar").style.width = "0";
}
// function startGame() {
//   let cardDealer = deck.shift(); //first Card
//   // console.log(cardDealer);
//   // console.log(deck);
// }
</script>

<template>
  <!-- home page click to start -->
  <!-- <div class="absolute w-full h-full" id="backgroundStart" @click="clickToStart" v-show="start">
    <h1 class="w-full flex justify-center mt-9 font-sans font-extrabold text-9xl tracking-wider text-white">BLACKJACK</h1>
    <h4 class="w-full font-sans font-semibold text-3xl flex justify-center text-end mt-96 text-gray-300">click to start</h4>
  </div> -->

  <!-- rule before playing -->
  <!-- <div @click="clickToStart" class="absolute w-full h-full bg-blue-400 z-30" v-show="color"></div> -->
  <!-- <div @click="clickToStart" class="absolute bg-red-300 z-10 p-40" v-show="rule"></div> -->

  <div class="w-screen h-screen">

    <!-- BG Fix -->
    <div class="bg-bottom bg-cover bg-no-repeat w-full h-full bg-[url('./src/assets/img/bg-fix-fix.png')]">
      <!-- BG Blur -->
      <div
        class="absolute w-screen h-screen z-20 bg-bottom bg-cover bg-no-repeat bg-[url('../src/assets/img/bg-blur.jpg')]"
        @click="bg_first_func" v-show="bg_first">
        <h1 class="w-full flex justify-center mt-9 
        font-sans font-extrabold text-9xl tracking-widest text-white">
          BLACKJACK
        </h1>
        <h4 class="w-full font-sans font-semibold text-3xl flex justify-center 
        text-end mt-64 text-gray-300">
          click to start
        </h4>
      </div>

      <!-- navbar -->
      <!-- <div class="slide-bar">
        <div id="slideBar"
          class="hidden w-96 fixed text-white z-50 h-screen top-0 right-0 bg-black overflow-hidden float-right ease-in duration-300"> -->
      <!-- bar -->
      <!-- <div>
            <ul>
              <li class="pb-16 mt-3"><a class="absolute left-5 text-5xl hover:text-slate-400 " href="javascript:void(0)"
                  v-on:click="closeNav()">×</a></li>
              <li><img class="w-24 m-auto" src="../src/assets/img/back_card.png"></li>
              <li><a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">RESUME</a></li>
              <li><a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">RULE</a></li>
              <li><a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">QUIT</a></li>
            </ul>
          </div> -->
      <!-- play music -->
      <!-- <div class="flex pr-2 pt-36 justify-center flex-row">
            <p class="text-2xl pr-3 -top-1.5">music</p>
            <audio class="hidden" controls id="song">
              <source src="" type="audio/ogg">
            </audio>
            <input type="range" min="1" max="100" value="50" id="volume" />
            <p id="on" class="text-2xl pl-5 -top-1.5 cursor-pointer text-orange-400" v-on:click="onMusic()">on</p>
            <p id="off" class="text-2xl pl-5 -top-1.5 cursor-pointer" v-on:click="offMusic()">off</p>

            <input type="text" name="on" v-model="onMusic">
            <input type="text" name="off" v-model="offMusic">
          </div>
        </div>
        <button id="setting"
          class="block text-7xl cursor-pointer text-white hover:text-slate-400 float-right right-3 top-3"
          v-on:click="openNav()">
          <SystemUiconsSettings />
        </button>
      </div> -->

      <div class="drawer drawer-end absolute">
        <!-- <button id="setting"
          class="block text-7xl cursor-pointer text-white hover:text-slate-400 float-right right-3 top-3"
          v-on:click="openNav()">
        </button> -->
        <!-- <setting id="my-drawer-4" class="drawer-toggle"/> -->
        <input id="my-drawer-4" type="checkbox" class="drawer-toggle flex justify-end" />
        <div class="drawer-content">
          <!-- Page content here -->
          <label for="my-drawer-4" class="drawer-button btn btn-primary float-right mt-3 mr-3">Open drawer</label>
        </div>
        <div class="drawer-side">
          <label for="my-drawer-4" class="drawer-overlay"></label>
          <ul class="menu p-4 w-80 bg-base-100 text-base-content">
            <!-- Sidebar content here -->
            <li class="pb-16 mt-3"><a class="absolute left-5 text-5xl hover:text-slate-400 " href="javascript:void(0)">×</a></li>
              <li><img class="w-24 m-auto" src="../src/assets/img/back_card.png"></li>
              <li><a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">RESUME</a></li>
              <li><a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">RULE</a></li>
              <li><a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">QUIT</a></li>
          </ul>
        </div>
      </div>
      <!-- Score -->
      <div class="flex justify-center items-center" v-show="!bg_first">
        <div class="relative flex justify-center">
          <div class="totalScore z-10 relative flex items-center justify-center "><span class="pt-10 "> Round: </span>
          </div>
          <div class="absolute top-0 m-auto z-30 round">
            <span class="flex justify-center items-center w-60 h-12 font-lg font-bold">1 - 0</span>
          </div>
          <div class="player absolute m-auto z-20 flex items-center top-0 bx6 justify-around">
            <div class="flex">PLAYER:</div>
            <div class="flex"></div>
            <div class="flex">DEALER:</div>
          </div>
        </div>
      </div>

      <!-- Btn How to Play -->
      <div class="w-full absolute" v-show="!bg_first">
        <label for="my-modal-4" class="btn w-14 rounded-full text-center align-middle font-bold text-2xl ml-10"> ?
        </label>

        <input type="checkbox" id="my-modal-4" class="modal-toggle" />
        <label for="my-modal-4" class="modal cursor-pointer">
          <label class="modal-box relative bg-white" for="">
            <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
            <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!
            </p>
          </label>
        </label>
      </div>


      <!-- Text How to play -->
      <div class="rule absolute w-2/5 h-4/5 bg-white opacity-90" v-show="how_to_play">
        <p class="pt-6 text-center text-3xl">HOW TO PLAY</p>
      </div>

      <!-- Dealer -->
      <div class="w-full pt-6">
        <div class="flex justify-center space-x-8">
          <img class="w-24" src='./assets/img/All-card-final/backcard/back_card.svg' ref="hiddenCard">
          <img class="w-24" src='./assets/img/All-card-final/7-C.png'>
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
          <img class="w-24" src="./assets/img/All-card-final/J-T.png">
          <img class="w-24" src="./assets/img/All-card-final/A-C.png">
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
