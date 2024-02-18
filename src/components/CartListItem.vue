<script setup>
import CartItem from '@/components/CartItem.vue'
</script>

<template>
  <div class="flex-col flex gap-4 justify-between">
    <CartItem :price="150" title="Man Sneakers Air Force 1" image-url="/sneakers/sneakers-7.jpg" />
    <CartItem :price="150" title="Man Sneakers Air Force 1" image-url="/sneakers/sneakers-7.jpg" />
    <CartItem :price="150" title="Man Sneakers Air Force 1" image-url="/sneakers/sneakers-7.jpg" />
    <CartItem :price="150" title="Man Sneakers Air Force 1" image-url="/sneakers/sneakers-7.jpg" />
  </div>
</template>

<style scoped></style>
