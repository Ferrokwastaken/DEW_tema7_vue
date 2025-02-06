<script setup>
import { ref } from 'vue'
import { computed } from 'vue'

const searchWord = ref('')

const props = defineProps({
  products: Array
})

const filteredProducts = computed(() => {
  return props.products.filter(product => {
    return product.name.toLocaleLowerCase().includes(searchWord.value.toLocaleLowerCase())
  })
})

defineEmits(['addToCart'])

</script>

<template>
  <h1>Listado de Productos</h1>
  <p>Filtrar por
    <input type="text" v-model="searchWord">
  </p>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Categoría</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="product in filteredProducts" :key="product.id">
        <td>{{ product.id }}</td>
        <td>{{ product.name }}</td>
        <td>{{ product.price }}</td>
        <td>{{ product.category }}</td>
        <td>
          <button @click="$emit('addToCart', product.id)">Añadir al carrito</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style>
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

  th {
    background-color: #333;
    color: #fff;
  }

  tr:nth-child(even) {
    background-color: #f2f2f2;
  }

  button {
    padding: 0.5rem;
    font-size: 1rem;
    background-color: #333;
    color: #fff;
    border: none;
    cursor: pointer;
  }
</style>