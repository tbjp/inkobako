<script setup>
import { useRouter } from "vue-router";
import { ref, onMounted } from "vue";
import ToolboxLink from "@/components/ToolboxLink.vue";
import blueinkoImg from "../assets/blueinko.webp";
import brainImg from "../assets/brain.webp";
import elephantImg from "../assets/elephant.webp";
import lightningImg from "../assets/lightning.webp";
import listImg from "../assets/list.webp";
import timerImg from "../assets/timer.webp";
import lotteryImg from "../assets/lottery.webp";

const router = useRouter();
const inkoHidden = ref(true); // Start hidden

// Show inko after 3 seconds when component mounts
onMounted(() => {
  setTimeout(() => {
    inkoHidden.value = false;
  }, 1000);
});

const hideInko = () => {
  inkoHidden.value = true;
  setTimeout(() => {
    inkoHidden.value = false;
  }, 60000); // 60000ms = 1 minute
};

const navigateToTimer = () => {
  router.push("/timer");
};

// Navigation functions for other routes
const navigateToMemory = () => {
  router.push("/memory");
};

const navigateToSleepingElephants = () => {
  router.push("/sleeping-elephants");
};

const navigateToStormSpeak = () => {
  router.push("/storm-speak");
};

const navigateToRandomList = () => {
  router.push("/random-list");
};

const navigateToLottery = () => {
  router.push("/lottery");
};
</script>

<template>
  <main class="p-4 pt-6 flex flex-col items-center gap-4 grow">
    <h1 class="font-daruma text-6xl text-blue-500 mb-4 text-center">インコばこ</h1>

    <div
      class="bg-amber-50 rounded-3xl p-8 border-4 border-orange-100 max-w-md mx-auto grow relative"
    >
      <img
        :src="blueinkoImg"
        alt="Blue Inko"
        @mouseover="hideInko"
        :class="[
          'w-42 h-42 md:w-64 md:h-64 absolute z-[-1] left-[-30px] transition-all duration-1000 ease-in-out',
          inkoHidden ? 'top-[250px]' : 'top-[-100px]',
        ]"
      />
      <div class="grid grid-cols-2 gap-8 p-2">
        <ToolboxLink :imageUrl="timerImg" label="Timer" @click="navigateToTimer" />
        <ToolboxLink
          :imageUrl="brainImg"
          label="Memory"
          @click="navigateToMemory"
          class="pointer-events-none"
        />
        <ToolboxLink
          :imageUrl="elephantImg"
          label="Sleeping Elephants"
          @click="navigateToSleepingElephants"
          class="pointer-events-none"
        />
        <ToolboxLink
          :imageUrl="lightningImg"
          label="Storm Speak"
          @click="navigateToStormSpeak"
          class="pointer-events-none"
        />
        <ToolboxLink
          :imageUrl="listImg"
          label="Random List"
          @click="navigateToRandomList"
          class="pointer-events-none"
        />
        <ToolboxLink
          :imageUrl="lotteryImg"
          label="Lottery"
          @click="navigateToLottery"
          class="pointer-events-none"
        />
      </div>
    </div>
  </main>
</template>
