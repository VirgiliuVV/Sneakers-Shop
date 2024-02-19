<script setup>
import { inject } from 'vue'
defineProps({
  totalPrice: Number
})
const { toggleDrawer } = inject('cart')

const openDrawer = () => {
  toggleDrawer()
}
</script>
<template>
  <header class="flex justify-between items-center border-b border-slate-200 px-10 py-8">
    <router-link to="/"
      ><div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-14" />
        <div>
          <h2 class="text-xl font-bold uppercase">Virgiliu's Sneakers Bazar</h2>
          <p class="text-slate-400">Best Sneakers Shop in Town</p>
        </div>
      </div></router-link
    >

    <ul class="flex items-center gap-10">
      <li
        @click="openDrawer"
        class="flex items-center gap-1 text-gray-500 hover:text-black cursor-pointer"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }}$</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center gap-1 text-gray-500 hover:text-black cursor-pointer">
          <img src="/heart.svg" alt="Favorite" />
          <span>Favorite</span>
        </li>
      </router-link>
    </ul>
  </header>
</template>
