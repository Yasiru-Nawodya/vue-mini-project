<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'
import ProductCard from './components/ProductCard.vue'

interface Product {
  id: number
  title: string
  price: number
  thumbnail: string
  category: string   // catogray interface
}

const products = ref<Product[]>([])
const searchQuery = ref('')

// selected catogray interface
const selectedCategory = ref('all') 

onMounted(async () => {
  const res = await fetch('https://dummyjson.com/products')
  const data = await res.json()
  products.value = data.products
})

// Computed unique categories for filtering

const categories = computed(() => {
  const all = products.value.map(p => p.category)
  return ['all', ...new Set(all)]
})

// Computed filtered products based on search and category

const filteredProducts = computed(() =>
  products.value.filter(product => {
    const matchSearch = product.title
      .toLowerCase()
      .includes(searchQuery.value.toLowerCase())

    const matchCategory =
      selectedCategory.value === 'all' ||
      product.category === selectedCategory.value

    return matchSearch && matchCategory
  })
)
</script>

<template>
  <div class="app">
    <h1 class="title">Our Products</h1>

    <!-- Search Bar -->
    <div class="search-container">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search products..."
        class="search-input"
      />
    </div>
    

    <!-- Category Filter Buttons -->

    <div class="category-container">
  <button
    v-for="cat in categories"
    :key="cat"
    @click="selectedCategory = cat"
    :class="['category-btn', selectedCategory === cat ? 'active' : '']"
  >
    {{ cat }}
  </button>
</div>

    <!-- Product Grid -->
    <div class="product-grid">
      <ProductCard
        v-for="product in filteredProducts"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>
</template>

<style>
/* Container */
.app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
  font-family: 'Arial', sans-serif;
  color: #1f1f1f;
}

/* Title */
.title {
  text-align: center;
  font-size: 2.5rem;
  font-weight: bold;
  margin-bottom: 1rem;
  color: #333;
}

/* Search Bar */
.search-container {
  display: flex;
  justify-content: center;
  margin-bottom: 2rem;
}

.search-input {
  width: 100%;
  max-width: 400px;
  padding: 0.6rem 1rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 25px;
  outline: none;
  transition: box-shadow 0.3s ease, border-color 0.3s ease;
}

.search-input:focus {
  border-color: #aa3bff;
  box-shadow: 0 0 8px rgba(170, 59, 255, 0.3);
}

/* Grid */
.product-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.5rem;
}

@media (max-width: 1024px) {
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 600px) {
  .product-grid {
    grid-template-columns: 1fr;
  }
}

/* ProductCard inline styling remains same as before */
.product-card {
  border: 1px solid #ddd;
  border-radius: 10px;
  overflow: hidden;
  text-align: center;
  background-color: #fff;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.product-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.15);
}

.product-card img {
  width: 100%;
  height: 180px;
  object-fit: cover;
  display: block;
}

.product-card h2 {
  font-size: 1.1rem;
  margin: 0.5rem 0;
  font-weight: 600;
}

.product-card p {
  color: #00a86b;
  font-weight: bold;
  margin-bottom: 1rem;
}


.category-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.category-btn {
  padding: 0.4rem 1rem;
  border: 1px solid #ccc;
  border-radius: 20px;
  background: white;
  cursor: pointer;
  transition: 0.3s;
}

.category-btn:hover {
  background: #f0f0f0;
}

.category-btn.active {
  background: #333;
  color: white;
  border-color: #333;
}

</style>