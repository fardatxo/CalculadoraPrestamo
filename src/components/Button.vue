<script setup>
// prop para recibir el operador desde el padre
const props = defineProps({
  operador: {
    type: String,
    required: true
  }
})

// el emit para enviar el click al padre
const emit = defineEmits(['clickBoton'])
</script>

<template>
    <button
        type="button"
        @click="$emit('clickBoton')"
        class="h-10 w-10 flex items-center justify-center font-bold
        bg-lime-500 rounded-full hover:outline-none hover:ring-2
        hover:ring-offset-2 hover:ring-lime-500 text-white text-2xl"> {{ operador }} </button>
</template>