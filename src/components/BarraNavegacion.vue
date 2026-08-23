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

const emitir = defineEmits(['alReiniciar'])

// ── Derivados ──
const estaAlInicio = computed(() => props.cartaActual === 1)

const porcentaje = computed(() =>
  Math.round((props.cartaActual / props.totalCartas) * 100)
)

// ── Manejadores ──
function manejarReiniciar() {
  emitir('alReiniciar')
}
</script>

<style scoped>
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

.barra__contador {
  font-size: 0.85rem;
  opacity: 0.8;
}

.barra__barra-visual {
  width: 100%;
  max-width: 300px;
  height: 6px;
  background-color: rgba(255, 255, 255, 0.15);
  border-radius: 3px;
  overflow: hidden;
}

.barra__relleno {
  height: 100%;
  background-color: #3b82f6;
  border-radius: 3px;
  transition: width 0.3s ease;
}
</style>
