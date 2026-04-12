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
.product-card {
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 1rem;
  text-align: center;
  transition: transform 0.2s, box-shadow 0.2s;
  background: #fff;
}

.product-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 18px rgba(0,0,0,0.2);
}

.product-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 6px;
  margin-bottom: 0.75rem;
}

.product-title {
  font-size: 1.1rem;
  font-weight: bold;
  margin: 0.5rem 0;
}

.product-price {
  font-size: 1rem;
  color: #2f855a;
  font-weight: 600;
}
</style>