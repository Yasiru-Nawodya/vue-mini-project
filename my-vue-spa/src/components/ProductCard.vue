<script setup lang="ts">
import type { Product } from '../types/product'


const emit = defineEmits(['select'])

defineProps<{ product: Product }>()
</script>

<template>
 
  <div class="product-card" @click="emit('select')">
    <img :src="product.thumbnail" alt="" class="product-image"/>
    <h2 class="product-title">{{ product.title }}</h2>
    <p class="product-price">${{ product.price }}</p>
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