<script setup>
import { useRouter } from "vue-router";
import ToolboxLink from "@/components/ToolboxLink.vue";

const router = useRouter();

const navigateToTimer = () => {
  router.push("/timer");
};
</script>

<template>
  <main class="p-4 pt-6 flex flex-col items-center gap-4 grow">
    <h1 class="font-daruma text-5xl text-blue-500 mb-8 text-center">インコばこ</h1>

    <div class="bg-amber-50 rounded-3xl p-8 border-4 border-orange-100 max-w-md mx-auto grow">
      <div class="grid grid-cols-2 gap-8 p-2 h-full">
        <ToolboxLink icon="⏰" label="Timer" @click="navigateToTimer" />
        <ToolboxLink icon="✓" label="Todo" iconColor="text-green-600" />
        <ToolboxLink icon="💤" label="Sleep" iconColor="text-blue-500" />
        <ToolboxLink icon="🌦️" label="Weather" iconColor="text-gray-600" />
      </div>
    </div>
  </main>
</template>
