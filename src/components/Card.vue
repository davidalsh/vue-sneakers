<script setup>
defineProps({
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
})
</script>

<template>
  <div class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
    <img v-if="onClickFavorite" :src="isFavorite ? '/like-2.svg' : '/like-1.svg'" alt="Like" @click="onClickFavorite" class="absolute top-8 left-8">
    <img :src="imageUrl" alt="Sneakers">

    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between">
      <div class="flex flex-col mt-5">
        <span class="text-slate-400">Price:</span>
        <b>{{ price }} $</b>
      </div>
      <img v-if="onClickAdd" :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="Add" @click="onClickAdd">
    </div>
  </div>
</template>
