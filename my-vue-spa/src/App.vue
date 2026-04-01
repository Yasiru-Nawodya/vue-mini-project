<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Product {
  id: number
  title: string
  price: number
  thumbnail: string
}

const products = ref<Product[]>([])

onMounted(async () => {
  const res = await fetch('https://dummyjson.com/products')
  const data = await res.json()
  products.value = data.products
})
</script>

<template>
  <div class="p-4">
    <h1 class="text-3xl font-bold mb-4">Products</h1>
    <div v-for="product in products" :key="product.id" class="mb-2">
      <p>{{ product.title }} - ${{ product.price }}</p>
    </div>
  </div>
</template>