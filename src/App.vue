<script setup>
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Card from '@/components/Card.vue'
</script>

<template>
  <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14">
    <Header />
    <div class="p-10">
      <h2 class="text-3xl font-bold mb-10">All Sneakers</h2>

      <CardList />
    </div>
  </div>
</template>

<style scoped></style>
