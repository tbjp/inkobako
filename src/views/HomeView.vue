<script setup>
import { useRouter } from "vue-router";

const router = useRouter();

const navigateToTimer = () => {
  router.push("/timer");
};
</script>

<template>
  <main class="p-4 pt-6 flex flex-col items-center gap-4">
    <h1 class="font-daruma text-5xl text-blue-500 mb-8 text-center">インコばこ</h1>

    <div class="bg-amber-50 rounded-3xl p-8 border-4 border-orange-100 max-w-md mx-auto">
      <div class="grid grid-cols-2 gap-8 p-2">
        <!-- Timer Card -->
        <div
          class="flex flex-col items-center gap-2 cursor-pointer scale-hover"
          @click="navigateToTimer"
        >
          <div class="text-6xl">⏰</div>
          <div class="text-xl font-normal font-daruma text-gray-800">Timer</div>
        </div>

        <!-- Todo List Card -->
        <div class="flex flex-col items-center gap-2 cursor-pointer scale-hover">
          <div class="text-6xl text-green-600">✓</div>
          <div class="text-xl font-normal font-daruma text-gray-800">Todo</div>
        </div>

        <!-- Sleep Tracker Card -->
        <div class="flex flex-col items-center gap-2 cursor-pointer scale-hover">
          <div class="text-6xl text-blue-500">💤</div>
          <div class="text-xl font-normal font-daruma text-gray-800">Sleep</div>
        </div>

        <!-- Weather Card -->
        <div class="flex flex-col items-center gap-2 cursor-pointer scale-hover">
          <div class="text-6xl text-gray-600">🌦️</div>
          <div class="text-xl font-normal font-daruma text-gray-800">Weather</div>
        </div>
      </div>
    </div>
  </main>
</template>
