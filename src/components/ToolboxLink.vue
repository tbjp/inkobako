<template>
  <div class="flex flex-col items-center gap-2 cursor-pointer scale-hover" @click="onClick">
    <div class="text-6xl" :class="iconColor">{{ icon }}</div>
    <div class="text-xl font-normal font-daruma text-gray-800">{{ label }}</div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const emit = defineEmits(["click"]);

defineProps({
  icon: {
    type: String,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
  iconColor: {
    type: String,
    default: "",
  },
});

const onClick = () => {
  emit("click");
};
</script>
