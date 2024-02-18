<script setup>
defineProps({
  imageUrl: String,
  title: String,
  price: Number
})
</script>

<template>
  <div class="gap-8 flex items-center border border-slate-200 p-4 rounded-xl">
    <img :src="imageUrl" alt="Sneaker" class="w-16 h-16" />
    <div class="flex flex-col">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b>{{ price }}$</b>
        <img
          class="transition cursor-pointer opacity-50 hover:opacity-100"
          src="/close.svg"
          alt="Delete"
        />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
