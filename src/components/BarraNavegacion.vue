<!--
  BarraNavegacion.vue
  ──────────────────────────────────────────────
  Componente de encabezado (header) de la aplicación.
  Muestra el título de la app, un indicador de progreso
  visual de cartas y un botón para reiniciar la partida.
  ──────────────────────────────────────────────
-->
<template>
  <header class="barra">
    <!-- Título de la app -->
    <h1 class="barra__titulo">{{ titulo }}</h1>

    <!-- Indicador de progreso -->
    <div class="barra__progreso">
      <span class="barra__contador">
        Carta {{ cartaActual }} de {{ totalCartas }}
      </span>
      <div class="barra__barra-visual">
        <div
          class="barra__relleno"
          :style="{ width: `${porcentaje}%` }"
        />
      </div>
    </div>

    <!-- Botón reiniciar -->
    <Boton
      variante="secundario"
      :estaDeshabilitado="estaAlInicio"
      @alHacerClic="manejarReiniciar"
    >
      Reiniciar
    </Boton>
  </header>
</template>

<script setup>
/**
 * BarraNavegacion - Encabezado de la aplicación.
 * @property {string} titulo - Nombre de la app que se muestra como encabezado.
 * @property {number} cartaActual - Índice de la carta visible (1-basado).
 * @property {number} totalCartas - Cantidad total de cartas del mazo.
 */
import { computed } from 'vue'
import Boton from './Boton.vue'

const props = defineProps({
  titulo: {
    type: String,
    required: true
  },
  cartaActual: {
    type: Number,
    required: true
  },
  totalCartas: {
    type: Number,
    required: true
  }
})

// Evento emitido cuando el usuario quiere reiniciar la partida
const emitir = defineEmits(['alReiniciar'])

// ── Derivados ──
/** Determina si estamos en la primera carta (deshabilita el botón Reiniciar) */
const estaAlInicio = computed(() => props.cartaActual === 1)

/** Calcula el porcentaje de progreso para la barra visual */
const porcentaje = computed(() =>
  Math.round((props.cartaActual / props.totalCartas) * 100)
)

// ── Manejadores ──
/** Notifica al padre que se debe reiniciar la partida */
function manejarReiniciar() {
  emitir('alReiniciar')
}
</script>

<style scoped>
/* ── Barra contenedora: layout flex horizontal ── */
.barra {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  padding: 0.8rem 1.6rem;
  background-color: #1e293b;
  color: #f8fafc;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}

/* ── Título ── */
.barra__titulo {
  font-size: 1.25rem;
  font-weight: 700;
  margin: 0;
  white-space: nowrap;
}

/* ── Progreso ── */
.barra__progreso {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
}

/* Texto del contador: "Carta X de Y" */
.barra__contador {
  font-size: 0.85rem;
  opacity: 0.8;
}

/* Contenedor de la barra visual (track) */
.barra__barra-visual {
  width: 100%;
  max-width: 300px;
  height: 6px;
  background-color: rgba(255, 255, 255, 0.15);
  border-radius: 3px;
  overflow: hidden;
}

/* Relleno dinámico: su ancho se controla con el computed "porcentaje" */
.barra__relleno {
  height: 100%;
  background-color: #3b82f6;
  border-radius: 3px;
  transition: width 0.3s ease;
}
</style>
