<script setup>

defineProps({
  cartSum: Number,
})

const emit = defineEmits(['onClickCart']);

</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">Shop of the best sneakers</p>
        </div>
      </div>
    </router-link>
    <ul class="flex items-center gap-10">
      <li @click="() => emit('onClickCart')" class="flex items-center cursor-pointer gap-2 text-gray-500 hover:text-black">
        <img src="/cart.svg" alt="Cart">
        <b>{{ cartSum }}$</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center cursor-pointer gap-2 text-gray-500 hover:text-black">

            <img src="/heart.svg" alt="Favorites">
            <span>Favorites</span>
        </li>
      </router-link>
      <li class="flex items-center cursor-pointer gap-2 text-gray-500 hover:text-black">
        <img src="/profile.svg" alt="Profile">
        <span>Profile</span>
      </li>
    </ul>
  </header>
</template>
