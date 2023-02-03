<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

</script>

<template>
  <div class="w-full">
    <!-- Dealer -->
    <div class="w-full py-6">
      <h1 class="flex justify-center font-bold text-xl">Dealer: 17</h1>
      <div class="flex justify-center space-x-5 pt-4">
        <img class="w-36 " src="./assets/img/All-card-final/8-A.png">
        <img class="w-36 " src="./assets/img/All-card-final/9-C.png">
      </div>
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
    <div class="w-full pt-3">
      <h1 class="flex justify-center font-bold text-xl">Player: 21</h1>
      <div class="flex justify-center space-x-5 pt-4">
        <img class="w-36 " src="./assets/img/All-card-final/J-T.png">
        <img class="w-36 " src="./assets/img/All-card-final/A-C.png">
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
