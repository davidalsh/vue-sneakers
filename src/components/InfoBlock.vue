<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String,
})
</script>

<template>
  <div class="flex flex-col gap-2 items-center text-center w-72 mx-auto">
    <img :src="imageUrl" alt="Info image" height="120" width="120">
    <h2 class="mt-2 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>

<style scoped>

</style>