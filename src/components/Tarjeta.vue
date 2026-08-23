<template>
  <div
    :class="[
      'tarjeta',
      `tarjeta--${palo}`,
      { 'tarjeta--boca-abajo': !bocaArriba },
      { 'tarjeta--seleccionada': estaSeleccionada }
    ]"
    @click="manejarClic"
  >
    <!-- Frente de la carta -->
    <div v-if="bocaArriba" class="tarjeta__frente">
      <span class="tarjeta__valor">{{ valor }}</span>
      <span class="tarjeta__letra">{{ letra }}</span>
    </div>

    <!-- Dorso de la carta -->
    <div v-else class="tarjeta__dorso">
      <span class="tarjeta__dorso-texto">?</span>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  valor: {
    type: Number,
    required: true
  },
  palo: {
    type: String,
    required: true,
    validador: (valor) =>
      ['corazones', 'diamantes', 'tréboles', 'picas'].includes(valor)
  },
  letra: {
    type: String,
    required: true
  },
  bocaArriba: {
    type: Boolean,
    default: true
  },
  estaSeleccionada: {
    type: Boolean,
    default: false
  }
})

const emitir = defineEmits(['alSeleccionar', 'alVoltear'])

function manejarClic() {
  if (!props.bocaArriba) {
    emitir('alVoltear')
  } else {
    emitir('alSeleccionar', {
      valor: props.valor,
      palo: props.palo,
      letra: props.letra
    })
  }
}
</script>

<style scoped>
.tarjeta {
  width: 120px;
  height: 170px;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  user-select: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

/* ── Frente: Palos con color ── */
.tarjeta--corazones .tarjeta__frente {
  background-color: #fef2f2;
  color: #dc2626;
  border: 2px solid #dc2626;
}

.tarjeta--diamantes .tarjeta__frente {
  background-color: #fff7ed;
  color: #ea580c;
  border: 2px solid #ea580c;
}

.tarjeta--tréboles .tarjeta__frente {
  background-color: #f0fdf4;
  color: #16a34a;
  border: 2px solid #16a34a;
}

.tarjeta--picas .tarjeta__frente {
  background-color: #f8fafc;
  color: #1e293b;
  border: 2px solid #1e293b;
}

/* ── Frente: Contenido ── */
.tarjeta__frente {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.4rem;
  width: 100%;
  height: 100%;
  border-radius: 8px;
}

.tarjeta__valor {
  font-size: 1.8rem;
  font-weight: 700;
}

.tarjeta__letra {
  font-size: 1.4rem;
  font-weight: 600;
  text-transform: uppercase;
}

/* ── Dorso ── */
.tarjeta--boca-abajo {
  background: linear-gradient(135deg, #1e3a5f 25%, #2563eb 50%, #1e3a5f 75%);
  border: 2px solid #1e3a5f;
}

.tarjeta__dorso {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  border-radius: 8px;
  background: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 5px,
    rgba(255, 255, 255, 0.06) 5px,
    rgba(255, 255, 255, 0.06) 10px
  );
}

.tarjeta__dorso-texto {
  font-size: 2rem;
  color: rgba(255, 255, 255, 0.5);
  font-weight: 700;
}

/* ── Estado: Seleccionada ── */
.tarjeta--seleccionada {
  transform: translateY(-6px);
  box-shadow: 0 0 0 3px #3b82f6, 0 4px 14px rgba(59, 130, 246, 0.4);
}

/* ── Hover ── */
.tarjeta:hover:not(.tarjeta--boca-abajo) {
  transform: translateY(-3px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}
</style>