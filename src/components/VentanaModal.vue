<template>
  <div v-if="estaAbierto" class="ventana-modal" @click.self="manejarCierreOverlay">
    <div class="ventana-modal__cuadro">
      <!-- Encabezado -->
      <div class="ventana-modal__encabezado">
        <h2 class="ventana-modal__titulo">{{ titulo }}</h2>
        <Boton
          variante="secundario"
          class="ventana-modal__boton-cerrar"
          @alHacerClic="manejarCierre"
        >
          ✕
        </Boton>
      </div>

      <!-- Cuerpo dinámico -->
      <div class="ventana-modal__cuerpo">
        <slot />
      </div>
    </div>
  </div>
</template>

<script setup>
import Boton from './Boton.vue'

defineProps({
  estaAbierto: {
    type: Boolean,
    required: true
  },
  titulo: {
    type: String,
    required: true
  }
})

const emitir = defineEmits(['alCerrar'])

function manejarCierre() {
  emitir('alCerrar')
}

function manejarCierreOverlay() {
  emitir('alCerrar')
}
</script>

<style scoped>
/* ── Overlay ── */
.ventana-modal {
  position: fixed;
  inset: 0;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(30, 41, 59, 0.7);
}

/* ── Cuadro de diálogo ── */
.ventana-modal__cuadro {
  background-color: #f8fafc;
  border-radius: 12px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  width: 90%;
  max-width: 520px;
  overflow: hidden;
}

/* ── Encabezado ── */
.ventana-modal__encabezado {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.2rem 1.5rem;
  border-bottom: 1px solid #e2e8f0;
}

.ventana-modal__titulo {
  margin: 0;
  font-size: 1.25rem;
  font-weight: 700;
  color: #1e293b;
}

.ventana-modal__boton-cerrar {
  padding: 0.3rem 0.7rem;
  font-size: 1.1rem;
  line-height: 1;
}

/* ── Cuerpo ── */
.ventana-modal__cuerpo {
  padding: 1.5rem;
  color: #1e293b;
}
</style>
