<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import axios from 'axios'
import Drawer from '@/components/Drawer.vue'

const items = ref([])
const cart = ref([])

const filters = reactive({
  searchQuery: '',
  sortBy: 'title'
})

const drawerOpen = ref(false)

const totalPrice = computed(() => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
})
const delivery = computed(() => {
  return totalPrice.value > 0
    ? totalPrice.value * 0.05 > 50
      ? 50
      : Math.trunc(totalPrice.value * 0.05)
    : 0
})

const toggleDrawer = () => {
  drawerOpen.value = !drawerOpen.value
}

const removeFromCart = (item) => {
  item.isAdded = false
  cart.value = cart.value.filter((el) => el.id !== item.id)
}

const addToCart = (item) => {
  item.isAdded = true
  cart.value.push(item)
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}
const addToFavorites = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post('https://81281dbfb5873512.mokky.dev/favorites', obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://81281dbfb5873512.mokky.dev/favorites/${item.favoriteId}`)
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

const changeSearchQuery = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  const { data: favorites } = await axios.get('https://81281dbfb5873512.mokky.dev/favorites')
  items.value = items.value.map((item) => {
    const isFavorite = favorites.find((favorite) => favorite.parentId === item.id)
    if (!isFavorite) return item
    return { ...item, isFavorite, favoriteId: isFavorite.id }
  })
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get(`https://81281dbfb5873512.mokky.dev/items`, { params })
    items.value = data.map((item) => ({
      ...item,
      isFavorite: false,
      isAdded: false,
      favoriteId: null
    }))
  } catch (error) {
    console.log(error)
  }
}
onMounted(() => {
  fetchItems()
  fetchFavorites()
})
watch(filters, () => {
  fetchItems()
})
provide('cart', { cart, toggleDrawer, addToCart, removeFromCart, onClickAddPlus })
</script>

<template>
  <Drawer v-if="drawerOpen" :total-price="totalPrice" :delivery="delivery" />
  <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" />
    <div class="p-10">
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
      <CardList :items="items" @addToFavorites="addToFavorites" @add-to-cart="onClickAddPlus" />
    </div>
  </div>
</template>

<style scoped></style>
