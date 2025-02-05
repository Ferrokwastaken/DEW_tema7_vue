<script setup>
import { ref } from 'vue'
import { onMounted } from 'vue'
import ListProducts from './components/ListProducts.vue'
import ShoppingCart from './components/ShoppingCart.vue'

const username = ref('Israel')
const products = ref([])
const cartIDs = ref([])

onMounted(async () => {
  const res = await fetch('http://localhost:3000/api/products')
  products.value = await res.json()
})

function addToCart(productId) {
  console.log('Añadir al carrito', productId)
  const cartID = cartIDs.value.find(element => element.id === productId)
  if (cartID) {
    cartID.amount++
  } else {
    cartIDs.value.push({ id: productId, amount: 1 })
  }
  console.log(cartIDs.value)
}

function removeFromCart (id) {
  cartIDs.value = cartIDs.value.filter(cartID => cartID.id !== id)
}

</script>

<template>
  <h1>Práctica Framework Vue3</h1>
  <p><input type="text" v-model="username"></p>
  <p>Elementos del carrito: {{ cartIDs.length }}</p>
  <p>Bienvenido {{ username }}</p>
  <ShoppingCart :cartIDs="cartIDs" @removeFromCart="removeFromCart"/>
  <ListProducts :products="products" @addToCart="addToCart"/>
</template>

<style scoped>
  h1 {
    color: #333;
  }

  p {
    color: #666;
  }

  input {
    padding: 0.5rem;
    font-size: 1rem;
  }
</style>
