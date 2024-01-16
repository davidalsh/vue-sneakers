<script setup>
import DrawerHead from '@/components/DrawerHead.vue'
import CartItemList from '@/components/CartItemList.vue'
import { computed, inject, ref } from 'vue'
import InfoBlock from '@/components/InfoBlock.vue'
import axios from 'axios'

const props = defineProps({
  cartSum: Number,
  isLoading: Boolean,
})

const cart = inject('cart')
const fee = 12
const isCreating = ref(false)
const orderId = ref(null)
const createOrder = async () => {
  try {
    isCreating.value = true;
    const { data } = await axios.post(`https://2e8db0244b60d9a1.mokky.dev/orders`, {
      items: cart.value,
      cartSum: props.cartSum,
    })
    cart.value = []

    orderId.value = data.id

    return data
  } catch (e) {
    console.log(e)
  } finally {
    isCreating.value = false;
  }
}

const finalAmount = computed(() => props.cartSum ? props.cartSum + fee : 0);
const buttonDisabled = computed(() => props.isLoading ? true : !props.cartSum);

</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div v-if="cartSum">
      <CartItemList />

      <div class="flex flex-col gap-2 mt-7">
        <div class="flex gap-2">
          <span>Summary:</span>
          <div class="flex-1 border-b-2 border-dotted"></div>
          <b>{{ cartSum }}$</b>
        </div>
        <div class="flex gap-2 text-slate-400">
          <span>fee*:</span>
          <div class="flex-1 border-b-2 border-dotted"></div>
          <b>{{ fee }}$</b>
        </div>
        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 disabled:bg-slate-300 cursor-pointer"
        >Pay {{ finalAmount }}$</button>
      </div>
    </div>
    <div v-else class="flex h-3/4 items-center">
      <InfoBlock v-if="orderId" title="Order completed" :description="`Your order #${ orderId } has been accepted successfully.`" image-url="/order-success-icon.png"/>
      <InfoBlock v-else title="Cart is empty" description="Add at least one item to the cart to make an order." image-url="/package-icon.png"/>
    </div>

  </div>
</template>