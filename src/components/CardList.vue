<script setup>
import Card from '@/components/Card.vue'

defineProps({
  items: Array
})
const emit = defineEmits(['addToFavorites', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :on-click-favorite="() => emit('addToFavorites', item)"
      :on-click-add="() => emit('addToCart', item)"
    />
  </div>
</template>

<style scoped></style>
