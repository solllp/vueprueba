<script setup>
const { producto } = defineProps({
  producto: Object
})

const emit = defineEmits(['comprar'])

function comprarProducto() {
  emit('comprar', producto)
}
</script>

<template>
  <div class="tarjeta">
    <h2>{{ producto.nombre }}</h2>

    <p>Precio: ${{ producto.precio }}</p>

    <p>Stock disponible: {{ producto.stock }}</p>

    <p v-if="producto.stock === 0" class="agotado">
      ❌ Producto agotado
    </p>

    <button
      @click="comprarProducto"
      :disabled="producto.stock === 0"
    >
      Comprar
    </button>
  </div>
</template>

<style scoped>
.tarjeta {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 16px;
}

.agotado {
  color: red;
  font-weight: bold;
}
</style>
