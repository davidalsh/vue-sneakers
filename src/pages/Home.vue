<script setup>
import CardList from '@/components/CardList.vue'
import axios from 'axios'
import { inject, onMounted, reactive, ref, watch } from 'vue'
import debounce from 'lodash.debounce'

const cart = inject('cart')
const items = ref([]);
const addToCart = inject('addToCart')
const addToFavorites = inject('addToFavorites')
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
});

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
}
const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value;
}, 400)

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://2e8db0244b60d9a1.mokky.dev/favorites')
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.item_id === item.id);

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    });
  } catch (e) {
    console.log(e);
  }
}
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get('https://2e8db0244b60d9a1.mokky.dev/items', {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }))
  } catch (e) {
    console.log(e);
  }
}

onMounted(async() => {
  const localCart = localStorage.getItem('cart');
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))

});

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }))
});

watch(filters, fetchItems);
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">All Sneakers</h2>

    <div class="flex gap-4">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="name">By name</option>
        <option value="price">By Price (Low to High)</option>
        <option value="-price">By Price (High to Low)</option>
      </select>

      <div class="relative">
        <img class="left-4 top-3 absolute" src="/search.svg" alt="Search">
        <input
          @input="onChangeSearchInput"
          type="text"
          placeholder="Search"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"/>
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList
      :items="items"
      @add-to-favorites="addToFavorites"
      @add-to-cart="addToCart"
    />
  </div>
</template>
