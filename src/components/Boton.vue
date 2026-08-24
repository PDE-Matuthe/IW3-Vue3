<!--
  Boton.vue
  ──────────────────────────────────────────────
  Componente botón genérico reutilizable.
  Acepta dos variantes visuales (primario y secundario)
  y un estado de deshabilitado. El contenido interno se
  inyecta mediante slot para máximo reuso.
  ──────────────────────────────────────────────
-->
<template>
  <!-- Clase dinámica según la variante: boton--primario | boton--secundario -->
  <button
    :class="['boton', `boton--${variante}`]"
    :disabled="estaDeshabilitado"
    @click="manejarClic"
  >
    <!-- Slot: permite inyectar texto o elementos hijos desde el padre -->
    <slot />
  </button>
</template>

<script setup>
/**
 * Props del componente Boton.
 * @property {string} variante - Estilo visual: 'primario' (default) o 'secundario'.
 * @property {boolean} estaDeshabilitado - Desactiva el botón cuando es true.
 */
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

// Evento personalizado que se emite al hacer clic
const emitir = defineEmits(['alHacerClic'])

/**
 * Manejador del clic: reenvía el evento original al componente padre.
 * @param {MouseEvent} evento - Evento nativo del DOM.
 */
function manejarClic(evento) {
  emitir('alHacerClic', evento)
}
</script>

<style scoped>
/* ── Estilos base del botón ── */
.boton {
  padding: 0.6rem 1.4rem;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  border: 2px solid transparent;
}

/* ── Variante primario: fondo sólido azul, texto blanco ── */
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