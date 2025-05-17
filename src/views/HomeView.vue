<script setup>
import { useRouter } from "vue-router";

const router = useRouter();

const navigateToTimer = () => {
  router.push("/timer");
};
</script>

<template>
  <main class="p-4 pt-6">
    <h1 class="font-daruma text-5xl text-blue-500 mb-8 text-center">インコばこ</h1>

    <div class="bg-amber-50 rounded-3xl p-8 shadow-md max-w-md mx-auto">
      <div class="grid grid-cols-2 gap-8 p-2">
        <!-- Timer Card -->
        <div
          class="flex flex-col items-center gap-2 cursor-pointer scale-hover"
          @click="navigateToTimer"
        >
          <div class="text-3xl">⏰</div>
          <div class="text-sm font-medium">Timer</div>
        </div>

        <!-- Todo List Card -->
        <div class="flex flex-col items-center gap-2 cursor-pointer scale-hover">
          <div class="text-3xl text-green-600">✓</div>
          <div class="text-sm font-medium">Todo</div>
        </div>

        <!-- Sleep Tracker Card -->
        <div class="flex flex-col items-center gap-2 cursor-pointer scale-hover">
          <div class="text-3xl text-blue-500">💤</div>
          <div class="text-sm font-medium">Sleep</div>
        </div>

        <!-- Weather Card -->
        <div class="flex flex-col items-center gap-2 cursor-pointer scale-hover">
          <div class="text-3xl text-gray-600">🌦️</div>
          <div class="text-sm font-medium">Weather</div>
        </div>
      </div>
    </div>
  </main>
</template>
