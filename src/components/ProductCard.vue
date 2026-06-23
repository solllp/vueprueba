<script setup>
const props = defineProps({
  producto: Object
})

const emit = defineEmits(['comprar'])

function comprarProducto() {
  emit('comprar', props.producto)
}
</script>

<template>
  <div class="tarjeta">
    <h2>{{ props.producto.nombre }}</h2>

    <p>Precio: ${{ props.producto.precio }}</p>

    <p>Stock disponible: {{ props.producto.stock }}</p>

    <p
      v-if="props.producto.stock === 0"
      class="agotado"
    >
      ❌ Producto agotado
    </p>

    <button
      @click="comprarProducto"
      :disabled="props.producto.stock === 0"
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
