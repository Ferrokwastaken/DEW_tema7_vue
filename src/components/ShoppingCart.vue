<script setup>

const props = defineProps({
  cartIDs: Array
})

defineEmits(['removeFromCart'])
</script>

<template>
  <h1>Carrito de la compra</h1>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Cantidad</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cartID in cartIDs" :key="cartID.id">
        <td>{{ cartID.id }}</td>
        <td>{{ cartID.amount }}</td>
        <td>
          <button @click="$emit('removeFromCart', cartID.id)">Eliminar</button>
        </td>
      </tr>
    </tbody>
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