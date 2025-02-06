<script setup>
import { computed, ref } from 'vue'
import { onMounted } from 'vue'
import ListProducts from './components/ListProducts.vue'
import ShoppingCart from './components/ShoppingCart.vue'

const username = ref('Israel')
const products = ref([])
const cartIDs = ref([])
const orderField = ref('name')

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

const sortProducts = computed(() => {
  if (orderField.value == 'name') {
    return products.value.sort((a, b) => a.name.localeCompare(b.name)) 
  } else {
    return products.value.sort((a, b) => a.price - b.price)
  }
})

async function deleteProduct(id) {
  console.log('Eliminando producto', id)
  try {
    const res = await fetch('http://localhost:3000/api/products/' + id, {
      method: 'DELETE'
    })
    if (res.ok) {
      console.log('Producto eliminado')
      removeFromCart(id)
      products.value = products.value.filter(p => p.id != id)
    } else {
      throw new Error("Error al eliminar el producto");
    }
  } catch (error) {
    console.log('Error de los gordos', error)
  }
}

</script>

<template>
  <h1>Práctica Framework Vue3</h1>
  <p><input type="text" v-model="username"></p>
  <p>Bienvenido {{ username }}</p>
  <p>Elementos del carrito: {{ cartIDs.length }}</p>
  <p>
    Ordenar por:
    <input type="radio" v-model="orderField" value="name" id="order-name">
    <label for="order-name">Nombre</label>
    <input type="radio" v-model="orderField" value="price" id="order-price">
    <label for="order-price">Precio</label>
  </p>
  <ShoppingCart :cartIDs="cartProducts" @removeFromCart="removeFromCart"
  @increaseAmount="increaseAmount" @decreaseAmount="decreaseAmount"
  />
  <ListProducts :products="sortProducts" @addToCart="addToCart"
  @deleteProduct="deleteProduct"
  />
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

  .warning {
    background-color: #f33;
  }
</style>
