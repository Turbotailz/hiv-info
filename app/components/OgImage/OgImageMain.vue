<script setup lang="ts">
defineOptions({
  inheritAttrs: false
})

defineProps({
  title: {
    type: String,
    required: true
  },
  description: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="w-full h-full flex flex-row bg-stone-900 px-8 py-16">
    <div class="flex w-2/12 mr-4">
      <img src="/android-chrome-512x512.png" width="200" height="200">
    </div>
    <div class="flex w-10/12 flex-col">
      <p class="text-4xl mt-0 text-red-500 uppercase font-bold">
        Red Ribbon
      </p>
      <h1 class="text-8xl m-0 text-white">
        {{ title }}
      </h1>
      <p class="text-5xl text-gray-200 leading-tight">
        {{ description }}
      </p>
    </div>
  </div>
</template>
