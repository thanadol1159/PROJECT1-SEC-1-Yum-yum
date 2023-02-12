<script setup>
import { ref } from "vue";
import SystemUiconsSettings from "./components/icons/SystemUiconsSettings.vue";

//variable HTML
let bg_first = ref(true);
const bg_first_func = () => { bg_first.value = false };
let showHit = ref(true);

// Variable JS
let deck = [], dealerArr = [], playerArr = [], firstCard = [];
// Player
let playerPoint = ref(0);
let playercountAce = ref(0);
let playerScorePoint = ref(0);
// Dealer
let dealerPoint = ref(0);
let dealercountAce = ref(0);
let dealerScorePoint = ref(0);
// Game
let numRound = ref(1);
let textWLB = ref("");
let onOff = ref(1);
let open = ref(false);

// Testing Case 
// let testA = ["K-A", "6-C", "10-N", "K-T", "10-C"]

// Window call function
window.onload = function () {
  builddeck();
  shuffledeck();
  startGame();
};
function builddeck() {
  let points = ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K",];
  let types = ["A", "C", "N", "T"];
  // let types = ["C", "D", "H", "S"];
  for (const type of types) {
    for (const point of points) {
      deck.push(point + "-" + type); // A-A , 2-A , ... , K-T
    }
  }
}
function shuffledeck() {
  for (let i = 0; i < deck.length; i++) {
    let j = Math.floor(Math.random() * deck.length); // random 52 card
    let temp = deck[i];
    deck[i] = deck[j];
    deck[j] = temp;
  }
}

function startGame() {
  // Dealer FirstCard
  drawCard(firstCard, dealerPoint, dealercountAce)
  // Dealer Card
  drawCard(dealerArr, dealerPoint, dealercountAce)
  for (let i = 0; i < 2; i++) {
    // Player Card
    drawCard(playerArr, playerPoint, playercountAce)
  }
  checkRule();
}

function getPicture(card) {
  let src = "img/All-card-final/" + card + ".png";
  // let src = 'testcard/cards/' + card + '.png';
  return src;
}

function getPoint(card, yourPoint, countAce) {
  let point = card.split("-")[0]; // "6-T" -> ["6", "T"]
  point == "A" ? yourPoint.value + 11 > 21 ? (point = 1) : ((point = 11), countAce.value++)
    : isNaN(point) ? (point = 10) : point;
  if (countAce.value == 1 && yourPoint.value + parseInt(point) > 21) {
    countAce.value--;
    point -= 10;
  }
  return parseInt(point);
}

function hit() {
  drawCard(playerArr, playerPoint, playercountAce)
  if (playerPoint.value === 21) {
    showHit.value = false;
    textWLB.value = "Black Jack"
    playerScorePoint.value++
  }
  else if (playerPoint.value > 21) {
    showHit.value = false;
    textWLB.value = "Player BUST";
    stay();
  }
}

function stay() {
  showHit.value = false;
  if (textWLB.value !== "Player BUST" && dealerPoint.value < 17) {
    for (let i = 1; dealerPoint.value < 17; i++) {
      drawCard(dealerArr, dealerPoint, dealercountAce)
      if (dealerPoint.value > 21) {
        textWLB.value = "Dealer BUST";
        playerScorePoint.value++;
      }
    }
  }
  checkRule();
}

function drawCard(arr, point, countAce) {
  let card = deck.shift();
  arr.push(getPicture(card));
  point.value += getPoint(card, point, countAce);
}

function checkRule() {
  if (playerPoint.value === 21 && dealerPoint.value === 21) {
    showHit.value = false;
    textWLB.value = "Tie"
  }
  else if (playerPoint.value === 21) {
    showHit.value = false;
    textWLB.value = "Black Jack"
    playerScorePoint.value++
  }
  else if (textWLB.value !== "Dealer BUST" && showHit.value === false) {
    if (textWLB.value === "Player BUST") {
      textWLB.value === "Player BUST"
      dealerScorePoint.value++;
    } else if (dealerPoint.value > playerPoint.value) {
      textWLB.value = "Dealer Win";
      dealerScorePoint.value++;
    } else if (dealerPoint.value < playerPoint.value) {
      textWLB.value = "Player Win";
      playerScorePoint.value++;
    } else if (dealerPoint.value === playerPoint.value) {
      textWLB.value = "Tie";
    }
  }
}

function newGame() {
  showHit.value = true;
  numRound.value++;
  resetEveryThing();
  startGame();
  if (deck.length <= 26) {
    deck = [];
    builddeck();
    shuffledeck();
  }
}
function resetEveryThing() {
  playerArr = [];
  dealerArr = [];
  firstCard = [];
  playerPoint.value = 0;
  dealerPoint.value = 0;
  textWLB.value = "";
}

function MenuOpen() {
  open.value = !open.value;
}

function onMusic() {
  song.play();
  onOff.value = 1;
}

function offMusic() {
  song.pause();
  onOff.value = 0;
}

function changeVolume() {
  let currentVolume = document.querySelector("#volume");
  let song = document.querySelector("#song");
  song.volume = currentVolume.value / 100;
}
</script>

<template>
  <div class="w-screen h-screen">
    <!-- BG Fix -->
    <div class="bg-bottom bg-cover bg-no-repeat w-full h-full bg-[url('img/bg-fix-fix.png')]">
      <!-- BG Blur -->
      <div class="absolute w-screen h-screen bg-bottom bg-cover bg-no-repeat bg-[url('img/bg-blur.jpg')]"
        @click="bg_first_func" v-show="bg_first">
        <h1 class="w-full flex justify-center mt-9 font-sans font-extrabold text-9xl tracking-wider text-white">
          BLACKJACK
        </h1>
        <h4 class="w-full font-sans font-semibold text-3xl flex justify-center text-end mt-64 text-gray-300">
          click to start
        </h4>
      </div>
      <!-- navbar -->
      <div class="slide-bar">
        <div id="slideBar"
          class="w-96 fixed text-white h-screen z-50 top-0 right-0 bg-black overflow-hidden float-right ease-in duration-300"
          :class="[open ? 'right-0' : 'right-[-100%]']">
          <!-- bar -->
          <div>
            <ul>
              <!--link bar -->
              <li class="pb-16 mt-3">
                <a class="absolute left-5 text-5xl hover:text-slate-400" href="javascript:void(0)"
                  @click="MenuOpen()">×</a>
              </li>
              <li>
                <img class="w-24 m-auto" src="../src/assets/img/back_card.png" />
              </li>
              <li>
                <a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400"
                  @click="MenuOpen()">RESUME</a>
              </li>
              <li>
                <a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">RULE</a>
              </li>
              <li>
                <a class="pt-8 text-center text-6xl text-slate-200 block hover:text-zinc-400" href="#">QUIT</a>
              </li>
            </ul>
          </div>
          <!-- play music -->
          <div class="flex pr-2 pt-36 justify-center flex-row">
            <p class="text-2xl pr-3 -top-1.5">music</p>

            <!-- changeVolume music -->
            <audio class="hidden" autoplay loop id="song">
              <source v-on:click="changeVolume()" src="../src/song/‘Yours’.mp3" type="audio/mpeg" />
            </audio>

            <input type="range" min="1" max="100" value="50" id="volume" @click="changeVolume()" />

            <!-- stop start music -->
            <label :class="onOff === 1 ? 'text-orange-400' : ' text-white'"
              class="text-2xl pl-5 -top-1.5 cursor-pointer" for="on">on</label>
            <input type="text" class="hidden" id="on" v-on:click="onMusic()" checked v-model="onOff" />

            <label :class="onOff === 0 ? 'text-orange-400' : ' text-white'"
              class="text-2xl pl-5 -top-1.5 cursor-pointer" for="off">off</label>
            <input type="text" class="hidden" id="off" v-on:click="offMusic()" checked v-model="onOff" />
          </div>
        </div>

        <!-- close bar -->
        <button id="setting"
          class="absolute block text-7xl cursor-pointer text-white hover:text-slate-400 float-right right-3 top-3"
          @click="MenuOpen()">
          <SystemUiconsSettings />
        </button>
      </div>

      <!-- Score -->
      <div class="flex justify-center items-center text-stone-900" v-show="!bg_first">
        <div class="relative flex justify-center">
          <div class="totalScore z-10 relative flex items-center justify-center">
            <span class="pt-10"> Round: {{ numRound }} </span>
          </div>
          <div class="absolute top-0 m-auto z-30 round">
            <span class="compare flex justify-center items-center w-60 text-2xl font-bold">
              <span class="text-teal-300 mr-4">{{ playerScorePoint }}</span> -
              <span class="text-rose-600 ml-4">{{ dealerScorePoint }} </span>
            </span>
          </div>
          <div class="player absolute m-auto z-20 flex items-center top-0 bx6 justify-around">
            <div class="flex">PLAYER: {{ playerPoint }}</div>
            <div class="flex w-16"></div>
            <div class="flex">
              DEALER: <span v-show="!showHit">{{ dealerPoint }}</span>
            </div>
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
          <img v-if="showHit" src="img/All-card-final/backcard/back_card.svg" class="w-32" />
          <img v-else :src="firstCard" class="w-32" />
          <img v-for="card in dealerArr" :src="card" class="w-32" />
          <!-- popup -->
          <div v-show="!showHit">
            {{ textWLB }}
          </div>
        </div>
      </div>
      <h1 class="flex justify-center text-white font-bold text-xl py-4"></h1>

      <!-- Button -->
      <div class="w-full flex justify-center space-x-8 h-8">
        <button type="button"
          class="px-6 bg-green-500 hover:bg-green-600 active:bg-green-800 text-white font-bold text-lg text-center rounded-lg"
          @click="hit" :disabled="!showHit" :class="
            showHit
              ? 'none'
              : 'bg-gray-500 hover:bg-gray-500 active:bg-gray-500 cursor-not-allowed'
          ">
          HIT
        </button>
        <button type="button"
          class="px-6 bg-red-500 hover:bg-red-600 active:bg-red-800 text-white font-bold text-lg text-center rounded-lg"
          @click="stay" :disabled="!showHit" :class="
            showHit
              ? 'none'
              : 'bg-gray-500 hover:bg-gray-500 active:bg-gray-500 cursor-not-allowed'
          ">
          STAY
        </button>
      </div>
      <div v-if="!showHit" class="flex justify-center mt-5">
        <button type="button"
          class="px-6 py-1 bg-blue-500 hover:bg-blue-600 active:bg-blue-800 text-white font-bold text-lg text-center rounded-lg "
          @click="newGame">
          New Game
        </button>
      </div>

      <!-- Player -->
      <div class="w-full">
        <h1 class="flex justify-center text-white font-bold text-xl py-4"></h1>
        <div class="flex justify-center space-x-5" ref="imgCardPlayer">
          <img v-for="card in playerArr" :src="card" class="w-32" />
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

input[type="range"] {
  appearance: none;
  height: 2px;
  margin: 10px;
}

input[type="range"]::-webkit-slider-thumb {
  /* ตัวจุดหาย */
  appearance: none;
  height: 1rem;
  width: 1rem;
  background: #ffc21b;
  border-radius: 50%;
  cursor: pointer;
}
</style>
