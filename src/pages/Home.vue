<script setup>
import CardList from '@/components/CardList.vue'
import axios from 'axios'
import { inject, onMounted, reactive, watch } from 'vue'
import debounce from 'lodash.debounce'

const props = defineProps({
  items: {
    type: Array,
    default: () => []
  }
})

const { cart, onClickAddPlus } = inject('cart')

const filters = reactive({
  searchQuery: '',
  sortBy: 'title'
})

const addToFavorites = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post(import.meta.env.VITE_API_URL+'/favorites', obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(import.meta.env.VITE_API_URL+`/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    item.isFavorite = !item.isFavorite
    console.log(error)
  }
}

const changeSortBy = (event) => {
  filters.sortBy = event.target.value
}

const changeSearchQuery = debounce(async (event) => {
  filters.searchQuery = event.target.value
}, 200)

watch(cart, () => {
  props.items.value = props.items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

onMounted(async () => {
  await Promise.all([fetchItems(), fetchFavorites()]) // Wait for both functions to complete
})

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `${filters.searchQuery}`
    }
    const { data } = await axios.get(import.meta.env.VITE_API_URL+`/items`, { params })
    // Initialize props.items.value if it's not already initialized
    console.log(data)
    props.items.value = data.map((item) => ({
      ...item,
      isAdded: false,
      isFavorite: Boolean(item.isFavorite),
      favoriteId: null
    }))
  } catch (error) {
    console.log(error)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(import.meta.env.VITE_API_URL+'/favorites')
    if (!props.items.value) {
      props.items.value = []
    }
    props.items.value = props.items.value.map((item) => {
      const isFavorite = favorites.find((favorite) => favorite.item_id === item.id)
      if (!isFavorite) return item
      return { ...item, isFavorite, favoriteId: isFavorite.id }
    })
  } catch (error) {
    console.log(error)
  }
}

watch(filters, () => {
  fetchItems()
})
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-10">All Sneakers</h2>

    <div class="flex gap-4">
      <select @change="changeSortBy" class="py-2 px-3 border rounded-xl outline-none">
        <option value="title">By name</option>
        <option value="price">Cheapest first</option>
        <option value="-price">Most expensive</option>
      </select>

      <div class="relative">
        <img src="/search.svg" alt="Search" class="absolute left-4 top-3" />
        <input
          @input="changeSearchQuery"
          placeholder="Search..."
          class="focus:border-slate-400 outline-none border border-slate-200 rounded-xl py-2 pl-10 pr-4"
        />
      </div>
    </div>
  </div>
  <CardList :items="items.value" @addToFavorites="addToFavorites" @add-to-cart="onClickAddPlus" />
</template>

<style scoped></style>
