<script setup>
import { computed, provide, ref, watch } from 'vue'
import axios from 'axios'

import Header from '@/components/Header.vue'
import Drawer from '@/components/Drawer.vue'

const cart = ref([]);
const drawerOpen = ref(false);
const cartSum = computed(() => cart.value.reduce((sum, item) => sum + item.price, 0));
const onClickCart = () => {
  drawerOpen.value = !drawerOpen.value
}

const addItemToCart = (item) => {
  item.isAdded = true;
  cart.value.push(item);
}
const deleteItemFromCart = (item) => {
  item.isAdded = false;
  cart.value.splice(cart.value.indexOf(item), 1)
}
const addToCart = (item) => {
  item.isAdded = !item.isAdded;
  if (item.isAdded) {
    addItemToCart(item);
  } else {
    deleteItemFromCart(item);
  }
}
const addToFavorites = async (item) => {
  try {
    item.isFavorite = !item.isFavorite;

    if (item.isFavorite) {
      const { data } = await axios.post('https://2e8db0244b60d9a1.mokky.dev/favorites', {
        item_id: item.id,
      })
      item.favoriteId = data.id;
    } else {
      await axios.delete(`https://2e8db0244b60d9a1.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null;
    }
  } catch (e) {
    console.log(e)
  }
}

watch(cart, () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {
    deep: true
  }
)

provide('onClickCart', onClickCart)
provide('deleteItemFromCart', deleteItemFromCart)
provide('addItemToCart', addItemToCart)
provide('addToFavorites', addToFavorites)
provide('addToCart', addToCart)
provide('cart', cart)
</script>

<template>
  <Drawer v-if="drawerOpen" :cart-sum="cartSum" @on-click-cart="onClickCart"/>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :cart-sum="cartSum" @onClickCart="onClickCart"/>

    <div class="p-10">
      <router-view></router-view>
    </div>

  </div>
</template>

<style scoped></style>
