<script setup>
const emit = defineEmits(['on-click-remove'])

defineProps({
  imageUrl: String,
  title: String,
  price: Number
})
</script>

<template>
  <div class="gap-8 flex items-center border border-slate-200 p-4 rounded-xl">
    <img :src="imageUrl" :alt="title" class="w-16 h-16" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }}$</b>
        <img
          @click="emit('on-click-remove')"
          class="transition cursor-pointer opacity-50 hover:opacity-100"
          src="/close.svg"
          alt="Delete"
        />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
