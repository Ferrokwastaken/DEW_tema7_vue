<script setup>
import { ref } from 'vue'
import { computed } from 'vue'
import { onMounted } from 'vue'

const searchWord = ref('')
const searchCategory = ref('')
const categories = ref([])

const props = defineProps({
  products: Array
})

const filteredProducts = computed(() => {
  return props.products.filter(product => {
    return product.name.toLowerCase().includes(searchWord.value.toLowerCase())
      && (searchCategory.value == "" || product.category == searchCategory.value)
  })
})

defineEmits(['addToCart'], ['deleteProduct'])

onMounted(async () => {
  const res = await fetch('http://localhost:3000/api/categories')
  categories.value = await res.json()
})

</script>

<template>
  <h1>Listado de Productos</h1>
  <p>Filtrar por
    <input type="text" v-model="searchWord">
  </p>
  <p>Seleccionar categoría
    <select v-model="searchCategory">
      <option value="">Todas</option>
      <option v-for="(category, index) in categories" :key="index">
        {{ category }}
      </option>
    </select>
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
      <tr v-for="product in filteredProducts" :key="product.id"
      :class = "{warning: product.category == 'Alcoholic'}"
      >
        <td>{{ product.id }}</td>
        <td>{{ product.name }}</td>
        <td>{{ product.price }}</td>
        <td>{{ product.category }}</td>
        <td>
          <button @click="$emit('addToCart', product.id)">Añadir al carrito</button>
          <button @click="$emit('deleteProduct', product.id)">Eliminar Producto</button>
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
    margin-right: 1px;
    margin-left: 1px;
  }

  tr.warning {
    background-color: #f33;
  }
</style>