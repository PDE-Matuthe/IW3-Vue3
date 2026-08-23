<template>
  <button
    :class="['boton', `boton--${variante}`]"
    :disabled="estaDeshabilitado"
    @click="manejarClic"
  >
    <slot />
  </button>
</template>

<script setup>
defineProps({
  variante: {
    type: String,
    default: 'primario',
    validador: (valor) => ['primario', 'secundario'].includes(valor)
  },
  estaDeshabilitado: {
    type: Boolean,
    default: false
  }
})

const emitir = defineEmits(['alHacerClic'])

function manejarClic(evento) {
  emitir('alHacerClic', evento)
}
</script>

<style scoped>
.boton {
  padding: 0.6rem 1.4rem;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  border: 2px solid transparent;
}

/* ── Variante: Primario ── */
.boton--primario {
  background-color: #3b82f6;
  color: #ffffff;
}
.boton--primario:hover:not(:disabled) {
  background-color: #2563eb;
}

/* ── Variante: Secundario ── */
.boton--secundario {
  background-color: transparent;
  color: #3b82f6;
  border-color: #3b82f6;
}
.boton--secundario:hover:not(:disabled) {
  background-color: #eff6ff;
}

/* ── Estado deshabilitado ── */
.boton:disabled {
  opacity: 0.45;
  cursor: not-allowed;
}
</style>