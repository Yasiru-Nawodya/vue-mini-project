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

/* Cart visibility state */           

const showCart = ref(false)

/*   Add cart     */
const cart = ref<{ product: Product; qty: number }[]>([])

/* add to cart mode */

const addToCart = (product: Product) => {
  const existing = cart.value.find(item => item.product.id === product.id)

  if (existing) {
    existing.qty++
  } else {
    cart.value.push({ product, qty: 1 })
  }
}

/*remove function*/

const removeFromCart = (id: number) => {
  cart.value = cart.value.filter(item => item.product.id !== id)
}

const products = ref<Product[]>([])
const searchQuery = ref('')

// selected catogray interface
const selectedCategory = ref('all') 

// For future use if we want to show product details
const selectedProduct = ref<Product | null>(null)

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

<div class="cart-info" @click="showCart = !showCart">
  🛒 Cart: {{ cart.length }}
</div>


  <div class="cart-box" v-if="showCart && cart.length > 0">
  <h3>Cart Items</h3>

  <div v-for="item in cart" :key="item.product.id" class="cart-item">
    {{ item.product.title }}
    x{{ item.qty }}

    <button @click="removeFromCart(item.product.id)">❌</button>
  </div>
</div>

<!-- Total price calculation -->

<p class="total">
  Total: $
  {{
    cart.reduce((sum, item) => sum + item.product.price * item.qty, 0)
  }}
</p>

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
        @select="selectedProduct = product"
      />
    </div>

    <!-- Product Details -->
    <!-- <pre>{{ selectedProduct }}</pre> -->
    <!-- Product Detail Modal -->
<div v-if="selectedProduct" class="modal-overlay" @click="selectedProduct = null">
  <div class="modal" @click.stop>
    <img :src="selectedProduct.thumbnail" class="modal-image" />

    <h2>{{ selectedProduct.title }}</h2>
    <p class="modal-price">${{ selectedProduct.price }}</p>

    <p class="modal-category">
     Category: {{ selectedProduct.category }}
    </p>

    <button class="close-btn" @click="selectedProduct = null">
      Close
    </button>

    <button class="add-cart-btn" @click="addToCart(selectedProduct!)">
        Add to Cart 🛒
    </button>



  </div>
</div>


  </div>
</template>

<style>
.total {
  margin-top: 1rem;
  font-weight: bold;
}

/*card box styles*/ 

.cart-box {
  background: #fff;
  border: 1px solid #ddd;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 10px;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  margin: 0.5rem 0;
  align-items: center;
}

.cart-item button {
  border: none;
  background: red;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  padding: 2px 6px;
}


/* Cart Info */


.cart-info {
  text-align: right;
  margin-bottom: 1rem;
  font-weight: bold;
  cursor: pointer;
}


/*cart button*/ 

.add-cart-btn {
  padding: 0.5rem 1rem;
  margin-top: 1rem;
  border: none;
  background: #00a86b;
  color: white;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.3s;
}

.add-cart-btn:hover {
  background: #008f5a;
}


/* Modal Category pargraph */
.modal-category {
  font-size: 0.9rem;
  color: #666;
  margin: 0.5rem 0;
}

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

/* Overlay background */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.6);
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Modal box */
.modal {
  background: white;
  padding: 2rem;
  border-radius: 16px;
  width: 320px;
  text-align: center;
  box-shadow: 0 20px 40px rgba(0,0,0,0.3);
}

/* Image */
.modal-image {
  width: 100%;
  height: 180px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 1rem;
}

/* Price */
.modal-price {
  color: #00a86b;
  font-weight: bold;
  margin: 1rem 0;
}

/* Close button */
.close-btn {
  padding: 0.5rem 1rem;
  border: none;
  background: #333;
  color: white;
  border-radius: 6px;
  cursor: pointer;
}

/* Animation */
@keyframes fadeIn {
  from { opacity: 0; transform: scale(0.9); }
  to { opacity: 1; transform: scale(1); }
}


</style>