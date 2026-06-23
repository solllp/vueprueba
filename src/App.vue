<script setup>
import { ref, computed } from 'vue'
import ProductCard from './components/ProductCard.vue'

const busqueda = ref('')

const productos = ref([
  { id: 1, nombre: 'Auto Eléctrico', precio: 399, stock: 5 },
  { id: 2, nombre: 'Teclado Mecánico', precio: 899, stock: 3 },
  { id: 3, nombre: 'Audífonos Gamer', precio: 1299, stock: 8 },
  { id: 4, nombre: 'Monitor 24 pulgadas', precio: 2499, stock: 2 }
])

const carrito = ref([])

const productosFiltrados = computed(() => {
  return productos.value.filter(producto =>
    producto.nombre.toLowerCase().includes(busqueda.value.toLowerCase())
  )
})

const total = computed(() => {
  return carrito.value.reduce((acumulado, item) => {
    return acumulado + item.precio * item.cantidad
  }, 0)
})

const totalArticulos = computed(() => {
  return carrito.value.reduce((acumulado, item) => {
    return acumulado + item.cantidad
  }, 0)
})

function comprar(producto) {
  if (producto.stock <= 0) {
    return
  }

  producto.stock--

  const existente = carrito.value.find(
    item => item.id === producto.id
  )

  if (existente) {
    existente.cantidad++
  } else {
    carrito.value.push({
      id: producto.id,
      nombre: producto.nombre,
      precio: producto.precio,
      cantidad: 1
    })
  }
}

function eliminarDelCarrito(indice) {
  const item = carrito.value[indice]

  const productoOriginal = productos.value.find(
    p => p.id === item.id
  )

  if (productoOriginal) {
    productoOriginal.stock += item.cantidad
  }

  carrito.value.splice(indice, 1)
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

    <h1>Tienda Demo en Vue</h1>

    <h2>🛒 Carrito</h2>

    <p>Total de artículos: {{ totalArticulos }}</p>
    <h3>Total: ${{ total }}</h3>

    <ul>
      <li
        v-for="(item, indice) in carrito"
        :key="item.id"
      >
        {{ item.nombre }}
        x{{ item.cantidad }}
        - ${{ item.precio * item.cantidad }}

        <button @click="eliminarDelCarrito(indice)">
          ❌ Eliminar
        </button>
      </li>
    </ul>

    <hr>

    
    <ProductCard
      v-for="producto in productosFiltrados"
      :key="producto.id"
      :producto="producto"
      @comprar="comprar"
    />
  </div>
</template>

<style scoped>
.contenedor {
  max-width: 600px;
  margin: 20px auto;
}

.busqueda {
  width: 100%;
  padding: 8px;
  margin-bottom: 20px;
}

button {
  margin-left: 10px;
  padding: 6px 10px;
}
</style>
