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
        <!-- Timer image from import -->
        <img :src="timerImg" alt="Timer" class="w-16 h-16 mb-2" />
        <h1>Timer</h1>
      </div>

      <div
        class="bg-transparent w-full h-full max-w-md sm:max-w-lg md:max-w-2xl py-10 md:p-10 flex flex-col items-center justify-between relative"
      >
        <!-- background with z-index and absolute position to be same size as parent -->
        <div
          class="absolute bg-inko-rust left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 w-[150vw] md:w-full h-full shape"
        ></div>

        <!-- Timer Mode Selector -->
        <div class="mb-4 max-w-xs w-content">
          <select
            v-model="selectedMode"
            class="select text-center w-full bg-transparent font-light text-white text-2xl border-0 shadow-none"
            @change="handleModeChange"
          >
            <option value="countdown">Countdown</option>
            <option value="stopwatch">Stopwatch</option>
            <option value="random">Random</option>
            <option value="team">Team</option>
          </select>
        </div>
        <!-- Timer Display based on selected mode -->

        <!-- COUNTDOWN MODE -->
        <div
          v-if="selectedMode === 'countdown'"
          class="text-5xl md:text-7xl text-white font-bold mb-6 md:mb-10 transition-transform flex items-center gap-2"
        >
          <div class="flex flex-col items-center">
            <div class="countdown font-mono">
              <span :style="{ '--value': minutes }" aria-live="polite" aria-label="minutes">{{
                minutes.toString().padStart(2, "0")
              }}</span>
            </div>
          </div>
          <span class="pb-5">:</span>
          <div class="flex flex-col items-center">
            <div class="countdown font-mono">
              <span :style="{ '--value': seconds }" aria-live="polite" aria-label="seconds">{{
                seconds.toString().padStart(2, "0")
              }}</span>
            </div>
          </div>
        </div>

        <!-- STOPWATCH MODE -->
        <div
          v-else-if="selectedMode === 'stopwatch'"
          class="text-5xl md:text-7xl text-white font-bold mb-6 md:mb-10 transition-transform flex items-center gap-2"
        >
          <div class="flex flex-col items-center">
            <div class="countdown font-mono">
              <span :style="{ '--value': stopwatchHours }" aria-live="polite" aria-label="hours">{{
                stopwatchHours.toString().padStart(2, "0")
              }}</span>
            </div>
            <div class="text-xs mt-1 font-medium">Hours</div>
          </div>
          <span class="pb-5">:</span>
          <div class="flex flex-col items-center">
            <div class="countdown font-mono">
              <span
                :style="{ '--value': stopwatchMinutes }"
                aria-live="polite"
                aria-label="minutes"
                >{{ stopwatchMinutes.toString().padStart(2, "0") }}</span
              >
            </div>
            <div class="text-xs mt-1 font-medium">Minutes</div>
          </div>
          <span class="pb-5">:</span>
          <div class="flex flex-col items-center">
            <div class="countdown font-mono">
              <span
                :style="{ '--value': stopwatchSeconds }"
                aria-live="polite"
                aria-label="seconds"
                >{{ stopwatchSeconds.toString().padStart(2, "0") }}</span
              >
            </div>
            <div class="text-xs mt-1 font-medium">Seconds</div>
          </div>
        </div>

        <!-- RANDOM TIMER MODE -->
        <div
          v-else-if="selectedMode === 'random'"
          class="text-5xl md:text-7xl text-white font-bold mb-6 md:mb-10 transition-transform"
        >
          <div
            v-if="randomTimerRunning"
            class="countdown font-mono flex flex-col items-center gap-2"
          >
            <span class="text-lg">
              Time remaining between {{ minTimeMinutes }}-{{ maxTimeMinutes }} minutes</span
            >
            <div v-if="targetTimeReached" class="text-4xl md:text-6xl animate-bounce mt-2">
              Time's up!
            </div>
            <div v-else class="flex items-center">
              <div class="w-8 h-8 bg-white rounded-full animate-pulse"></div>
            </div>
            <div class="text-lg mt-4 flex flex-col items-center">
              <span class="text-base">Elapsed Time:</span>
              <div class="flex items-center gap-1">
                <div class="flex flex-col items-center">
                  <div class="countdown font-mono">
                    <span
                      :style="{ '--value': elapsedMinutes }"
                      aria-live="polite"
                      aria-label="minutes"
                    >
                      {{ elapsedMinutes.toString().padStart(2, "0") }}
                    </span>
                  </div>
                  <div class="text-xs mt-1 font-medium">Min</div>
                </div>
                <span class="pb-5">:</span>
                <div class="flex flex-col items-center">
                  <div class="countdown font-mono">
                    <span
                      :style="{ '--value': elapsedSeconds }"
                      aria-live="polite"
                      aria-label="seconds"
                    >
                      {{ elapsedSeconds.toString().padStart(2, "0") }}
                    </span>
                  </div>
                  <div class="text-xs mt-1 font-medium">Sec</div>
                </div>
              </div>
            </div>
          </div>
          <div v-else class="text-center">
            <div class="text-2xl">Random Timer</div>
            <div class="text-lg">
              Will stop between {{ minTimeMinutes }}-{{ maxTimeMinutes }} minutes (not yet
              implemented)
            </div>
          </div>
        </div>

        <!-- TEAM MODE -->
        <div
          v-else-if="selectedMode === 'team'"
          class="w-full text-gray-800 mb-2 md:mb-2 transition-transform"
        >
          <div v-for="team in teams" :key="team.id" class="flex flex-row mb-4 w-full">
            <div class="p-2 bg-opacity-10 rounded-lg bg-white w-full">
              <div class="flex justify-between items-center mb-1 w-full">
                <input
                  v-model="team.name"
                  class="bg-transparent text-gray-800 text-xl font-bold px-1 py-2 focus:outline-black w-[10em]"
                  :disabled="isRunning"
                />
                <div class="flex items-center text-2xl md:text-4xl font-mono gap-1">
                  <div class="flex flex-col items-center">
                    <div class="countdown font-mono">
                      <span
                        :style="{ '--value': team.minutes }"
                        aria-live="polite"
                        aria-label="minutes"
                      >
                        {{ team.minutes.toString().padStart(2, "0") }}
                      </span>
                    </div>
                    <div class="text-xs mt-1 font-medium">Min</div>
                  </div>
                  <span class="pb-5">:</span>
                  <div class="flex flex-col items-center">
                    <div class="countdown font-mono">
                      <span
                        :style="{ '--value': team.seconds }"
                        aria-live="polite"
                        aria-label="seconds"
                      >
                        {{ team.seconds.toString().padStart(2, "0") }}
                      </span>
                    </div>
                    <div class="text-xs mt-1 font-medium">Sec</div>
                  </div>
                </div>
              </div>
            </div>
            <div
              class="flex justify-center items-center min-w-[100px] transition-all duration-300 ease-in-out"
              :class="{
                'scale-100 pointer-events-auto': isRunning,
                'scale-0 pointer-events-none': !isRunning,
              }"
            >
              <button
                v-if="!team.isPaused"
                @click="pauseTeamTimer(team.id)"
                class="px-4 py-4 bg-white text-inko-rust rounded-full text-sm font-semibold"
              >
                <!-- Square white div -->
                <div class="w-4 h-4 bg-inko-rust"></div>
              </button>
              <div
                v-else-if="team.isPaused"
                class="px-4 py-4 bg-green-500 text-gray-800 rounded-full text-sm font-semibold"
              >
                Done!
              </div>
            </div>
          </div>
          <button
            @click="addTeam"
            class="btn btn-sm me-2 btn-circle btn-ghost bg-white text-inko-rust font-bold text-lg transition-all duration-300 ease-in-out"
            :class="{
              'scale-100 pointer-events-auto': !isRunning,
              'scale-0 pointer-events-none': isRunning,
            }"
          >
            +
          </button>
          <!-- Remove team button -->
          <button
            v-if="teams.length > 2"
            @click="removeTeam"
            class="btn btn-sm btn-circle btn-ghost bg-white text-inko-rust font-bold text-lg transition-all duration-300 ease-in-out"
            :class="{
              'scale-100 pointer-events-auto': !isRunning,
              'scale-0 pointer-events-none': isRunning,
            }"
          >
            -
          </button>
        </div>

        <!-- User Input sections for each mode -->

        <!-- COUNTDOWN MODE INPUT -->
        <div
          v-if="selectedMode === 'countdown'"
          :class="{
            'scale-0 pointer-events-none opacity-0': isRunning,
            'scale-100 pointer-events-auto opacity-100': !isRunning,
          }"
          class="flex gap-4 md:gap-8 mb-4 transition-all duration-300 ease-in-out"
        >
          <div>
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
            <h3 class="text-white text-xs text-center pt-1">Minutes</h3>
          </div>
          <div>
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
            <h3 class="text-white text-xs text-center pt-1">Seconds</h3>
          </div>
        </div>

        <!-- RANDOM TIMER INPUT -->
        <div
          v-if="selectedMode === 'random' && !randomTimerRunning"
          class="flex gap-4 md:gap-8 mb-4 transition-all duration-300 ease-in-out"
        >
          <div>
            <div class="bg-white rounded-lg p-2 md:p-3 text-center w-20 md:w-28 shadow-sm">
              <div
                class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
                @click="minTimeMinutes++"
              >
                ↑
              </div>
              <input
                type="text"
                class="text-2xl md:text-4xl font-bold text-center w-full bg-transparent border-none focus:outline-none"
                v-model="minTimeMinutes"
                maxlength="2"
              />
              <div
                class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
                @click="minTimeMinutes > 1 ? minTimeMinutes-- : null"
              >
                ↓
              </div>
            </div>
            <h3 class="text-white font-bold text-center pt-1">Min (min)</h3>
          </div>
          <div>
            <div class="bg-white rounded-lg p-2 md:p-3 text-center w-20 md:w-28 shadow-sm">
              <div
                class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
                @click="maxTimeMinutes++"
              >
                ↑
              </div>
              <input
                type="text"
                class="text-2xl md:text-4xl font-bold text-center w-full bg-transparent border-none focus:outline-none"
                v-model="maxTimeMinutes"
                maxlength="2"
              />
              <div
                class="text-gray-400 text-sm cursor-pointer hover:text-gray-600 active:text-gray-800 transition-colors"
                @click="maxTimeMinutes > minTimeMinutes ? maxTimeMinutes-- : null"
              >
                ↓
              </div>
            </div>
            <h3 class="text-white font-bold text-center pt-1">Max (min)</h3>
          </div>
        </div>

        <!-- CONTROL BUTTONS FOR ALL MODES -->
        <div class="flex gap-4 w-full justify-center mt-2">
          <button
            @click="handleStartStop()"
            class="w-24 py-3 bg-white rounded-full font-medium hover:bg-gray-100 transition-colors shadow-sm"
          >
            {{ isRunning ? "Stop" : "Start" }}
          </button>
          <button
            v-if="!isRunning"
            @click="handleReset()"
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
import { ref, onBeforeUnmount, reactive } from "vue";
import timerImg from "../assets/timer.webp";

// Timer mode state
const selectedMode = ref("countdown");

// Common timer state
const minutes = ref(1);
const seconds = ref(0);
const minutesInput = ref(1);
const secondsInput = ref(0);
const isRunning = ref(false);
let timerInterval = null;

// Stopwatch state
const stopwatchSeconds = ref(0);
const stopwatchMinutes = ref(0);
const stopwatchHours = ref(0);

// Random timer state
const minTimeMinutes = ref(1);
const maxTimeMinutes = ref(5);
const targetTimeSeconds = ref(0);
const elapsedTimeSeconds = ref(0);
const elapsedMinutes = ref(0);
const elapsedSeconds = ref(0);
const randomTimerRunning = ref(false);
const targetTimeReached = ref(false);

// Team timer state
const teams = reactive([
  { name: "Team 1", minutes: 0, seconds: 0, isRunning: false, isPaused: false, id: 1 },
  { name: "Team 2", minutes: 0, seconds: 0, isRunning: false, isPaused: false, id: 2 },
]);

// Handler for mode changes
const handleModeChange = () => {
  stopAllTimers();
  resetCurrentMode();
};

// Start/Stop Handler based on current mode
const handleStartStop = () => {
  if (isRunning.value) {
    stopAllTimers();
  } else {
    switch (selectedMode.value) {
      case "countdown":
        startCountdownTimer();
        break;
      case "stopwatch":
        startStopwatchTimer();
        break;
      case "random":
        startRandomTimer();
        break;
      case "team":
        startTeamTimer();
        break;
    }
  }
};

// Reset handler based on current mode
const handleReset = () => {
  switch (selectedMode.value) {
    case "countdown":
      resetCountdownTimer();
      break;
    case "stopwatch":
      resetStopwatchTimer();
      break;
    case "random":
      resetRandomTimer();
      break;
    case "team":
      resetTeamTimer();
      break;
  }
};

// Stop all timers and reset running state
const stopAllTimers = () => {
  clearInterval(timerInterval);
  isRunning.value = false;
  randomTimerRunning.value = false;
};

// Reset current mode settings
const resetCurrentMode = () => {
  switch (selectedMode.value) {
    case "countdown":
      resetCountdownTimer();
      break;
    case "stopwatch":
      resetStopwatchTimer();
      break;
    case "random":
      resetRandomTimer();
      break;
    case "team":
      resetTeamTimer();
      break;
  }
};

// ===== COUNTDOWN TIMER FUNCTIONS =====
const startCountdownTimer = () => {
  if (!isRunning.value) {
    if (minutes.value === 0 && seconds.value === 0) {
      resetCountdownTimer();
    }
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

const resetCountdownTimer = () => {
  stopAllTimers();
  minutes.value = minutesInput.value;
  seconds.value = secondsInput.value;
  formatInputs();
};

// ===== STOPWATCH TIMER FUNCTIONS =====
const startStopwatchTimer = () => {
  console.log("Starting stopwatch timer");
  if (!isRunning.value) {
    isRunning.value = true;

    // Continue from current values instead of resetting
    timerInterval = setInterval(() => {
      console.log(
        `Updating stopwatch: ${stopwatchHours.value}:${stopwatchMinutes.value}:${stopwatchSeconds.value}`,
      );
      stopwatchSeconds.value++;
      if (stopwatchSeconds.value === 60) {
        stopwatchSeconds.value = 0;
        stopwatchMinutes.value++;
      }
      if (stopwatchMinutes.value === 60) {
        stopwatchMinutes.value = 0;
        stopwatchHours.value++;
      }
    }, 1000);
  }
};

const resetStopwatchTimer = () => {
  stopAllTimers();
  stopwatchHours.value = 0;
  stopwatchMinutes.value = 0;
  stopwatchSeconds.value = 0;
};

// ===== RANDOM TIMER FUNCTIONS =====
const startRandomTimer = () => {
  if (!randomTimerRunning.value) {
    // Ensure max time is greater than min time
    if (maxTimeMinutes.value <= minTimeMinutes.value) {
      maxTimeMinutes.value = minTimeMinutes.value + 1;
    }

    // Calculate random time between min and max (in seconds)
    const minSeconds = minTimeMinutes.value * 60;
    const maxSeconds = maxTimeMinutes.value * 60;
    targetTimeSeconds.value =
      Math.floor(Math.random() * (maxSeconds - minSeconds + 1)) + minSeconds;

    targetTimeReached.value = false;
    randomTimerRunning.value = true;
    isRunning.value = true;

    elapsedTimeSeconds.value = 0;
    timerInterval = setInterval(() => {
      elapsedTimeSeconds.value++;
      elapsedMinutes.value = Math.floor(elapsedTimeSeconds.value / 60);
      elapsedSeconds.value = elapsedTimeSeconds.value % 60;

      if (elapsedTimeSeconds.value >= targetTimeSeconds.value) {
        targetTimeReached.value = true;
        clearInterval(timerInterval);
      }
    }, 1000);
  }
};

const resetRandomTimer = () => {
  stopAllTimers();
  targetTimeReached.value = false;
  elapsedTimeSeconds.value = 0;
  elapsedMinutes.value = 0;
  elapsedSeconds.value = 0;
};

// ===== TEAM TIMER FUNCTIONS =====
const addTeam = () => {
  if (!isRunning.value) {
    const newId = teams.length ? Math.max(...teams.map((t) => t.id)) + 1 : 1;
    teams.push({
      name: `Team ${newId}`,
      minutes: 0,
      seconds: 0,
      isRunning: false,
      isPaused: false,
      id: newId,
    });
  }
};

// Remove the last team if there are more than 2 teams
const removeTeam = () => {
  if (!isRunning.value && teams.length > 2) {
    teams.pop();
  }
};

const startTeamTimer = () => {
  if (!isRunning.value) {
    // Reset and prepare all teams
    teams.forEach((team) => {
      team.minutes = 0;
      team.seconds = 0;
      team.isPaused = false;
      team.isRunning = true;
    });

    isRunning.value = true;
    timerInterval = setInterval(() => {
      // Increment all non-paused team timers
      teams.forEach((team) => {
        if (!team.isPaused) {
          team.seconds++;
          if (team.seconds === 60) {
            team.seconds = 0;
            team.minutes++;
          }
        }
      });

      // If all teams are paused, stop the timer
      if (teams.every((team) => team.isPaused)) {
        clearInterval(timerInterval);
        isRunning.value = false;
      }
    }, 1000);
  }
};

const resetTeamTimer = () => {
  stopAllTimers();
  teams.forEach((team) => {
    team.minutes = 0;
    team.seconds = 0;
    team.isPaused = false;
    team.isRunning = false;
  });
};

const pauseTeamTimer = (teamId) => {
  const team = teams.find((t) => t.id === teamId);
  if (team) {
    team.isPaused = true;
  }
};

// ===== ORIGINAL COUNTDOWN UI FUNCTIONS =====
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
  minutesInput.value = parseInt(minutesInput.value, 10).toString().padStart(2, "0");
  secondsInput.value = parseInt(secondsInput.value, 10).toString().padStart(2, "0");
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

const formatSecondsInput = () => {
  const val = parseInt(secondsInput.value, 10);
  if (!isNaN(val) && val >= 60) {
    secondsInput.value = "59";
  }
};

// Initialize timer and format inputs
resetCountdownTimer();

// Clean up intervals when component is unmounted
onBeforeUnmount(() => {
  clearInterval(timerInterval);
});
</script>
