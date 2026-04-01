<script setup lang="ts">
import type { Product } from '../types/product'

defineProps<{
  product: Product
}>()
</script>

<template>
  <div class="border p-4 rounded shadow hover:shadow-lg transition">
    <img :src="product.thumbnail" alt="" class="w-full h-48 object-cover rounded"/>
    <h2 class="text-lg font-bold mt-2">{{ product.title }}</h2>
    <p class="text-green-500 font-semibold">${{ product.price }}</p>
  </div>
</template>