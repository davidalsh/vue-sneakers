<script setup>
import { inject } from 'vue'

defineProps({
  title: String,
  imageUrl: String,
  price: Number,
})
const emit = defineEmits(['deleteItemFromCart'])
</script>

<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" alt="Sneakers">

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b>{{ price }}$</b>
        <img
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="Delete"
          @click="emit('deleteItemFromCart')"
        />
      </div>
    </div>
  </div>
</template>
