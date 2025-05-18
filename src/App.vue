<script setup>
import { RouterView } from "vue-router";
</script>

<template>
  <div class="min-h-screen bg-green-100 pb-8 w-full flex flex-col">
    <router-view v-slot="{ Component, route }">
      <transition :name="route.meta.transition || 'home'">
        <component :is="Component" class="absolute inset-0" />
      </transition>
    </router-view>
  </div>
</template>

<style>
.tool-enter-active {
  transition: all 0.2s ease;
  position: absolute;
  width: 100%;
}

.tool-leave-active {
  position: absolute;
  transition: all 0.5s ease;
  width: 100%;
}

.tool-enter-from {
  opacity: 1;
  transform: scale(0);
}

.tool-leave-to {
  opacity: 0;
}

.home-enter-active,
.home-leave-active {
  transition: all 0.2s ease;
}

.home-enter-from {
  opacity: 0;
}

.home-leave-to {
  opacity: 0;
  transform: scale(0);
}
</style>
