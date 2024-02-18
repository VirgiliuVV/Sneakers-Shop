<script setup>
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from '@/components/Drawer.vue'
</script>

<template>
  <!--  <Drawer />-->
  <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14">
    <Header />
    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-10">All Sneakers</h2>

        <div class="flex gap-4">
          <select class="py-2 px-3 border rounded-xl outline-none">
            <option value="name">By name</option>
            <option value="cheap">Cheapest first</option>
            <option value="expensive">Most expensive</option>
          </select>

          <div class="relative">
            <img src="/search.svg" alt="Search" class="absolute left-4 top-3" />
            <input
              placeholder="Search..."
              class="focus:border-slate-400 outline-none border border-slate-200 rounded-xl py-2 pl-10 pr-4"
            />
          </div>
        </div>
      </div>
      <CardList />
    </div>
  </div>
</template>

<style scoped></style>
