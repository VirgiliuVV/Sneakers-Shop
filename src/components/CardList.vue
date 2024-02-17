<script setup>
import Card from '@/components/Card.vue'
</script>

<template>
  <div class="grid grid-cols-3 gap-5">
    <Card
      imageUrl="/sneakers/sneakers-1.jpg"
      title="Man Sneaker Air Jordan 1"
      :price="120"
      :isAdded="true"
      :isFavorite="true"
    />
  </div>
</template>

<style scoped></style>
