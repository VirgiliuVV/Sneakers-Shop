<script setup>
import DrawerHead from './DrawerHead.vue'
import CartListItem from '@/components/CartListItem.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />
    <CartListItem />

    <div class="flex flex-col gap-3 mb-5 mt-7">
      <div class="flex gap-2">
        <span>Total price:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>690$</b>
      </div>

      <div class="flex gap-2">
        <span>Delivery:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>20$</b>
      </div>
      <button
        disabled
        class="mt-3 cursor-pointer disabled:bg-slate-300 w-full bg-lime-500 text-white font-bold uppercase hover:bg-lime-600 active:bg-lime-700 transition py-3 rounded-3xl"
      >
        Checkout
      </button>
    </div>
  </div>
</template>

<style scoped></style>
