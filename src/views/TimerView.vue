<template>
  <div class="p-6">
    <div class="flex flex-col items-center justify-center h-full">
      <router-link
        to="/"
        class="self-start mb-4 text-gray-800 font-medium hover:text-gray-600 transition-colors"
      >
        &larr; Back
      </router-link>

      <div
        class="font-daruma text-4xl mb-4 flex flex-col items-center text-center text-neutral-800"
      >
        <div class="text-6xl">⏰</div>
        <h1>Timer</h1>
      </div>

      <div
        class="bg-inko-rust w-full h-full gap-4 shape max-w-md sm:max-w-lg md:max-w-2xl p-6 md:p-10 flex flex-col items-center justify-center"
      >
        <!-- Countdown display -->
        <div class="text-5xl md:text-7xl text-white font-bold mb-6 md:mb-10 transition-transform">
          <div class="countdown font-mono">
            <span :style="{ '--value': minutes }" aria-live="polite" aria-label="minutes">{{
              minutes.toString().padStart(2, "0")
            }}</span>
          </div>
          :
          <div class="countdown font-mono">
            <span :style="{ '--value': seconds }" aria-live="polite" aria-label="seconds">{{
              seconds.toString().padStart(2, "0")
            }}</span>
          </div>
        </div>

        <!-- User Input -->
        <div
          :class="{
            'scale-0 pointer-events-none opacity-0': isRunning,
            'scale-100 pointer-events-auto opacity-100': !isRunning,
          }"
          class="flex gap-4 md:gap-8 mb-4 transition-all duration-300 ease-in-out"
        >
          <div class="bg-white rounded-lg p-2 md:p-3 text-center w-20 md:w-28 shadow-sm">
            <div
              class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
              @click="incrementMinutes"
            >
              ↑
            </div>
            <input
              type="text"
              class="text-2xl md:text-4xl font-bold text-center w-full bg-transparent border-none focus:outline-none"
              v-model="minutesInput"
              maxlength="2"
              @blur="updateMinutes"
            />
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
            <input
              type="text"
              class="text-2xl md:text-4xl font-bold text-center w-full bg-transparent border-none focus:outline-none"
              v-model="secondsInput"
              maxlength="2"
              @blur="updateSeconds"
              @input="formatSecondsInput"
            />
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
import { ref } from "vue";
const minutes = ref(1);
const seconds = ref(0);
const minutesInput = ref(1);
const secondsInput = ref(0);
const isRunning = ref(false);
let timerInterval = null;

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
  minutes.value = minutesInput.value;
  seconds.value = secondsInput.value;
};

const incrementMinutes = () => {
  minutesInput.value++;
  if (!isRunning.value) minutes.value = minutesInput.value;
  formatInputs();
};

const decrementMinutes = () => {
  if (minutesInput.value > 0) {
    minutesInput.value--;
    if (!isRunning.value) minutes.value = minutesInput.value;
  }
  formatInputs();
};

const incrementSeconds = () => {
  if (secondsInput.value === 59) {
    secondsInput.value = 0;
    minutesInput.value++;
  } else {
    secondsInput.value++;
  }
  if (!isRunning.value) {
    minutes.value = minutesInput.value;
    seconds.value = secondsInput.value;
  }
  formatInputs();
};

const decrementSeconds = () => {
  if (secondsInput.value === 0) {
    if (minutesInput.value > 0) {
      secondsInput.value = 59;
      minutesInput.value--;
    }
  } else {
    secondsInput.value--;
  }
  if (!isRunning.value) {
    minutes.value = minutesInput.value;
    seconds.value = secondsInput.value;
  }
  formatInputs();
};

const formatInputs = () => {
  minutesInput.value = minutesInput.value.toString().padStart(2, "0");
  secondsInput.value = secondsInput.value.toString().padStart(2, "0");
};

const updateMinutes = () => {
  const parsedMinutes = parseInt(minutesInput.value, 10);
  if (!isNaN(parsedMinutes) && parsedMinutes >= 0 && parsedMinutes <= 99) {
    minutes.value = parsedMinutes;
  } else {
    minutesInput.value = minutes.value.toString().padStart(2, "0");
  }
  formatInputs();
};

const updateSeconds = () => {
  const parsedSeconds = parseInt(secondsInput.value, 10);
  if (!isNaN(parsedSeconds) && parsedSeconds >= 0 && parsedSeconds <= 59) {
    seconds.value = parsedSeconds;
  } else {
    secondsInput.value = seconds.value.toString().padStart(2, "0");
  }
  formatInputs();
};

// Initialize timer and format inputs
resetTimer();
formatInputs();
</script>
