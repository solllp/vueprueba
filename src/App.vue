<script setup>
import ProductCard from './components/ProductCard.vue'
import { ref, computed } from 'vue'

const busqueda = ref('')

const productos = ref([
  { id: 1, nombre: 'Auto Eléctrico', precio: 399, stock: 5 },
  { id: 2, nombre: 'Teclado Mecánico', precio: 899, stock: 3 },
  { id: 3, nombre: 'Audífonos Gamer', precio: 1299, stock: 8 },
  { id: 4, nombre: 'Monitor 24 pulgadas', precio: 2499, stock: 2 }
])

const productosFiltrados = computed(() => {
  return productos.value.filter(producto =>
    producto.nombre.toLowerCase().includes(busqueda.value.toLowerCase())
  )
})

function comprar(producto) {
  if (producto.stock > 0) {
    producto.stock--
  }
}
</script>

<template>

  <div class="contenedor">
    <input
      v-model="busqueda"
      type="text"
      placeholder="Buscar producto..."
      class="busqueda"
    >
    <h1>Tienda Demo en Vue</h1><br>

    <ProductCard
      v-for="producto in productosFiltrados"
      :key="producto.id" :producto="producto" @comprar="comprar"
    />
  </div>
</template>

//Estilos

<style scoped>


.contenedor {
  max-width: 500px;
  margin: 20px auto;
}

.tarjeta {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 16px;
}

button {
  padding: 8px 14px;
}

.busqueda {
  width: 100%;
  padding: 8px;
  margin-bottom: 40px;
}

.agotado {
  margin: 10px;
  color: yellow;
  font-size: 2vh;
  font-weight: bold;
}
</style>
