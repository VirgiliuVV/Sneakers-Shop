<script setup>
import { inject } from 'vue'
import CartItem from '@/components/CartItem.vue'

const { cart, removeFromCart } = inject('cart')

const onClickRemove = (item) => removeFromCart(item)
</script>

<template>
  <div class="flex-col flex gap-4 justify-between" v-auto-animate>
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :price="item.price"
      :title="item.title"
      :image-url="item.imageUrl"
      :is-favorite="item.isFavorite"
      @on-click-remove="onClickRemove(item)"
    />
  </div>
</template>

<style scoped></style>
