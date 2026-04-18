<script setup lang="ts">
import type { Product } from '../types/product'


const emit = defineEmits(['select'])

defineProps<{ product: Product }>()
</script>

<template>
  <div
    class="bg-white rounded-xl shadow p-4 cursor-pointer transform transition duration-300 hover:-translate-y-2 hover:shadow-xl"
    @click="emit('select')"
  >

    <!-- IMAGE -->
    <div class="overflow-hidden rounded-xl bg-gray-100">
      <img
        :src="product.thumbnail"
        alt=""
        class="w-full h-48 object-contain transition duration-300 hover:scale-105"
      />
    </div>

    <!-- TITLE -->
    <h2 class="mt-3 text-lg font-semibold">
      {{ product.title }}
    </h2>

    <!-- PRICE -->
    <p class="text-gray-600">
      ${{ product.price }}
    </p>

  </div>
</template>
<style>

.product-category {
  font-size: 0.8rem;
  color: #888;
}


.product-card {
  border: none;
  border-radius: 12px;
  overflow: hidden;
  text-align: center;
  background: #fff;
  transition: all 0.3s ease;
  cursor: pointer;
}

.product-card:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 15px 30px rgba(0,0,0,0.2);
}

.product-card img {
  width: 100%;
  height: 180px;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.product-card:hover img {
  transform: scale(1.08);
}

.product-title {
  font-size: 1.1rem;
  font-weight: bold;
  margin: 0.5rem 0;
}

.product-price {
  font-size: 1.2rem;
  color: #00a86b;
  font-weight: bold;
  margin-bottom: 1rem;
}
</style>