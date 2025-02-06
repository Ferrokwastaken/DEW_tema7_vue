<script setup>
import { computed, ref } from 'vue'
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

function increaseAmount (id) {
  const cartID = cartIDs.value.filter(cartID => cartID.id === id)[0]
  cartID.amount++
}

function decreaseAmount(id) {
  const cartID = cartIDs.value.filter(cartID => cartID.id === id)[0]
  cartID.amount--
  if (cartID.amount === 0) {
    removeFromCart(id)
  }
}

const cartProducts = computed(() => {
  return cartIDs.value.map(item => {
    const product = products.value.filter(p => p.id === item.id)[0]
    item.name = product.name
    item.price = product.price
    item.subtotal = item.amount * product.price
    return item
  })
})

</script>

<template>
  <h1>Práctica Framework Vue3</h1>
  <p><input type="text" v-model="username"></p>
  <p>Bienvenido {{ username }}</p>
  <p>Elementos del carrito: {{ cartIDs.length }}</p>
  <ShoppingCart :cartIDs="cartProducts" @removeFromCart="removeFromCart"
  @increaseAmount="increaseAmount" @decreaseAmount="decreaseAmount"
  />
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
