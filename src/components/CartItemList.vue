<script setup>
import CartItem from '@/components/CartItem.vue'
import { inject } from 'vue'

const cart = inject('cart')
const deleteItemFromCart = inject('deleteItemFromCart')
</script>

<template>
  <div class="flex flex-col gap-4" v-auto-animate>
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      @delete-item-from-cart="() => deleteItemFromCart(item)"
    />
  </div>
</template>
