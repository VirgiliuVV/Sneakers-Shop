<script setup>
import { inject, onMounted, ref } from 'vue'
import axios from 'axios'
import CardList from '@/components/CardList.vue'

const favorites = ref([])

const { onClickAddPlus } = inject('cart')

const addToCart = (item) => {
  onClickAddPlus(item)
}

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://81281dbfb5873512.mokky.dev/favorites?_relations=items'
    )
    favorites.value = data.map((favorite) => favorite.item)
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-10">My Favorites</h2>
  <CardList :items="favorites" @add-to-cart="addToCart" />
</template>

<style scoped></style>
