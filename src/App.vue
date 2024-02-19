<script setup>
import { computed, provide, ref } from 'vue'
import Header from '@/components/Header.vue'
import Drawer from '@/components/Drawer.vue'
import Home from '@/pages/Home.vue'

const cart = ref([])
const items = ref([])

const isCreatingOrder = ref(false)

const createOrder = async () => {
  isCreatingOrder.value = true
  await new Promise((resolve) => setTimeout(resolve, 3000))
  cart.value = []
  isCreatingOrder.value = false
  drawerOpen.value = false
  items.value.forEach((item) => {
    if (item.isAdded) {
      item.isAdded = false
    }
  })
}

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
  const index = cart.value.findIndex((el) => el.id === item.id)
  if (index !== -1) {
    cart.value.splice(index, 1)
  }
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

provide('cart', {
  cart,
  toggleDrawer,
  addToCart,
  removeFromCart,
  onClickAddPlus,
  createOrder
})
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :is-creating-order="isCreatingOrder"
    :delivery="delivery"
  />
  <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" />
    <div class="p-10">
      <router-view :items="items" />
    </div>
  </div>
</template>

<style scoped></style>
