<script setup>
import {computed} from 'vue'

const props = defineProps({
  cartIDs: Array
})

defineEmits(['removeFromCart', 'increaseAmount', 'decreaseAmount'])

const total = computed(() => {
  return props.cartIDs.reduce((total, item) => total + item.subtotal, 0)
})
</script>

<template>
  <h1>Carrito de la compra</h1>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Cantidad</th>
        <th>Subtotal</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cartID in cartIDs" :key="cartID.id">
        <td>{{ cartID.id }}</td>
        <td>{{ cartID.name }}</td>
        <td>{{ parseFloat(cartID.price).toFixed(2) }}€</td>
        <td>{{ cartID.amount }}
          <button @click="$emit('decreaseAmount', cartID.id)">-</button>
          <input type="number" v-model="cartID.amount" min="1">
          <button @click="$emit('increaseAmount', cartID.id)">+</button>
        </td>
        <td>{{ cartID.subtotal.toFixed(2) }}€</td>
        <td>
          <button @click="$emit('removeFromCart', cartID.id)">Eliminar</button>
        </td>
      </tr>
    </tbody>
    <tfoot>
      <td colspan="4">Total:</td>
      <td>{{ total.toFixed(2) }}€</td>
      <td></td>
    </tfoot>
  </table>
</template>

<style scoped>
  h1 {
    color: #333;
  }

  table {
    width: 100%;
    border-collapse: collapse;
  }

  th, td {
    border: 1px solid #333;
    padding: 8px;
    text-align: center;
  }
</style>