<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import CardList from '@/components/CardList.vue'
const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get('https://2e8db0244b60d9a1.mokky.dev/favorites?_relations=items')
    favorites.value = data.map(obj => ({
        ...obj.item,
        isFavorite: true,
      })
    )
  } catch (err) {
    console.log(err);
  }
});
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">My Favorites</h2>
  <CardList
    v-if="favorites.value"
    :items="favorites"
    is-favorites
  />
  <div v-else>
    <h3 class="text-slate-300">Nothing here yet...</h3>
  </div>
</template>
