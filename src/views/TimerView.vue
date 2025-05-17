<template>
  <div class="p-6">
    <div class="flex flex-col items-center justify-center">
      <router-link
        to="/"
        class="self-start mb-4 text-green-800 font-medium hover:text-green-600 transition-colors"
      >
        &larr; Back
      </router-link>

      <h1 class="font-daruma text-3xl mb-4 flex items-center gap-2 text-center text-neutral-800">
        <span class="text-3xl">⏰</span>
        Timer
      </h1>

      <div
        class="bg-red-500 rounded-[40px] w-full max-w-md sm:max-w-lg md:max-w-2xl p-6 md:p-10 flex flex-col items-center justify-center shadow-md"
      >
        <div class="text-5xl md:text-7xl text-white font-bold mb-6 md:mb-10">
          {{ displayTime }}
        </div>

        <div class="flex gap-4 md:gap-8 mb-4">
          <div class="bg-white rounded-lg p-2 md:p-3 text-center w-20 md:w-28 shadow-sm">
            <div
              class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
              @click="incrementMinutes"
            >
              ↑
            </div>
            <div class="text-2xl md:text-4xl font-bold">
              {{ minutes.toString().padStart(2, "0") }}
            </div>
            <div
              class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
              @click="decrementMinutes"
            >
              ↓
            </div>
          </div>

          <div class="bg-white rounded-lg p-2 md:p-3 text-center w-20 md:w-28 shadow-sm">
            <div
              class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
              @click="incrementSeconds"
            >
              ↑
            </div>
            <div class="text-2xl md:text-4xl font-bold">
              {{ seconds.toString().padStart(2, "0") }}
            </div>
            <div
              class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
              @click="decrementSeconds"
            >
              ↓
            </div>
          </div>
        </div>

        <div class="flex gap-4 w-full justify-center mt-6">
          <button
            @click="isRunning ? stopTimer() : startTimer()"
            class="w-24 py-3 bg-white rounded-full font-medium hover:bg-gray-100 transition-colors shadow-sm"
          >
            {{ isRunning ? "Stop" : "Start" }}
          </button>
          <button
            v-if="!isRunning"
            @click="resetTimer"
            class="w-24 py-3 bg-white rounded-full font-medium hover:bg-gray-100 transition-colors shadow-sm"
          >
            Reset
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
const minutes = ref(10);
const seconds = ref(5);
const isRunning = ref(false);
let timerInterval = null;

const displayTime = computed(() => {
  // Ensure minutes and seconds are always displayed with two digits
  return `${minutes.value.toString().padStart(2, "0")}:${seconds.value.toString().padStart(2, "0")}`;
});

const startTimer = () => {
  if (!isRunning.value) {
    isRunning.value = true;
    timerInterval = setInterval(() => {
      if (seconds.value > 0) {
        seconds.value--;
      } else if (minutes.value > 0) {
        minutes.value--;
        seconds.value = 59;
      } else {
        clearInterval(timerInterval);
        isRunning.value = false;
      }
    }, 1000);
  }
};

const stopTimer = () => {
  clearInterval(timerInterval);
  isRunning.value = false;
};

const resetTimer = () => {
  stopTimer();
  minutes.value = 10;
  seconds.value = 5;
};

const incrementMinutes = () => {
  minutes.value++;
};

const decrementMinutes = () => {
  if (minutes.value > 0) minutes.value--;
};

const incrementSeconds = () => {
  if (seconds.value === 59) {
    seconds.value = 0;
    minutes.value++;
  } else {
    seconds.value++;
  }
};

const decrementSeconds = () => {
  if (seconds.value === 0) {
    if (minutes.value > 0) {
      seconds.value = 59;
      minutes.value--;
    }
  } else {
    seconds.value--;
  }
};

// Initialize timer
resetTimer();
</script>
