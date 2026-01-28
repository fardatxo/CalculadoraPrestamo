<script setup lang="ts">
import { ref, computed } from 'vue'
import Header from './components/Header.vue';
import Button from './components/Button.vue';

// reactvos
const cantidad = ref(10000)
const plazo = ref(12)

// esta funcion cambia a moneda de euro
const formatearMoneda = (valorAformatear: number) => {
  return Number(valorAformatear).toLocaleString('es-ES', {
    style: 'currency',
    currency: 'EUR'
  })
}

// sube y baja 100
const decrementar = () => {
  const step = 100
  if (cantidad.value - step >= 0) {
    cantidad.value -= step
  }
}

const incrementar = () => {
  const step = 100
  if (cantidad.value + step <= 20000) {
    cantidad.value += step
  }
}

// computed para calcular el recargo por cantidad
const recargoImporte = computed(() => {
  if (cantidad.value <= 5000) return 1.50 // 50% recargo
  if (cantidad.value <= 10000) return 1.40 // 40% recargo  
  if (cantidad.value <= 15000) return 1.30 // 30% recargo
  return 1.20 // 20% recargo para más de 15000
})

// computed para calcular el recargo por plzo
const recargoPlazo = computed(() => {
  if (plazo.value === 6) return 1.10 // 10% recargo
  if (plazo.value === 12) return 1.20 // 20% recargo
  return 1.30 // 30% recargo para 24 meses
})

// Computed para el total
const importeTotal = computed(() => {
  return cantidad.value * recargoImporte.value * recargoPlazo.value
})

// Computed para el mensual
const importeMensual = computed(() => {
  return importeTotal.value / plazo.value
})
</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
  
    <Header/>
    <div class="flex justify-between mt-10">
        <Button operador="-" @clickBoton="decrementar"/>
        <Button operador="+" @clickBoton="incrementar"/>
    </div>
    <div class="my-5">
      <input
          type="range"
          v-model="cantidad"
          class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
          min="0"
          max="20000"
          step="100"
      />
      <p class="text-center my-10 text-5xl font-extrabold text-indigo-600">{{ formatearMoneda(cantidad) }}</p>
    </div>
    <h2 class="text-2xl font-extrabold text-gray-500 text-center"> 
      Elige un <span class="text-indigo-600">Plazo </span> a pagar
    </h2>
    <select
      v-model="plazo"
      class="w-full p-2 bg-gray-200 border border-gray-300 rounded-lg
      text-center text-xl font-bold text-gray-600 mt-5"
      >
      <option value="6">6 meses</option>
      <option value="12">12 meses</option>
      <option value="24">24 meses</option>
    </select>
    
    <h2 class="text-2xl font-extrabold text-gray-500 text-center">
      <br>Añade una <span class="text-indigo-600">Cantidad y Plazo </span> a pagar
    </h2>
    <div class="my-5 space-y-3 bg-gray-50 p-5">
        <h2 class="text-2xl font-extrabold text-gray-500 text-center">
            Resumen <span class="text-indigo-600">de pagos:</span>
        </h2>
        <p class="text-xl text-gray-500 text-center font-bold">{{ plazo }} Meses</p>
        <p class="text-xl text-gray-500 text-center font-bold">Importe Mensual: {{ formatearMoneda(importeMensual) }}</p>
        <p class="text-xl text-gray-500 text-center font-bold">Importe Total: {{ formatearMoneda(importeTotal) }}</p>

    </div>
</div> 
</template>

<style scoped>

</style>
