<template>
  <div class="aplicacion">
    <!-- Barra de navegación -->
    <BarraNavegacion
      titulo="MnemoCoach"
      :cartaActual="indiceActual + 1"
      :totalCartas="mazo.length"
      @alReiniciar="reiniciarPartida"
    />

    <!-- Contenido principal -->
    <main class="aplicacion__contenido">
      <!-- Carta actual -->
      <Tarjeta
        v-if="cartaActual"
        :valor="cartaActual.valor"
        :palo="cartaActual.palo"
        :letra="cartaActual.letra"
      />

      <!-- Botón siguiente carta -->
      <Boton
        variante="primario"
        :estaDeshabilitado="false"
        @alHacerClic="siguienteCarta"
      >
        Siguiente Carta
      </Boton>
    </main>

    <!-- Modal de victoria -->
    <VentanaModal
      :estaAbierto="mostrarModal"
      titulo="¡Victoria!"
      @alCerrar="reiniciarPartida"
    >
      <div class="victoria">
        <p class="victoria__mensaje">
          Has completado todas las cartas del mazo.
        </p>
        <Boton
          variante="primario"
          @alHacerClic="reiniciarPartida"
        >
          Volver a jugar
        </Boton>
      </div>
    </VentanaModal>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import BarraNavegacion from './components/BarraNavegacion.vue'
import Tarjeta from './components/Tarjeta.vue'
import Boton from './components/Boton.vue'
import VentanaModal from './components/VentanaModal.vue'

// ── Mazo de prueba ──
const mazo = ref([
  { valor: 1,  palo: 'corazones', letra: 't' },
  { valor: 2,  palo: 'diamantes', letra: 'n' },
  { valor: 3,  palo: 'tréboles',  letra: 'm' },
  { valor: 4,  palo: 'picas',     letra: 'r' },
  { valor: 5,  palo: 'corazones', letra: 'l' },
  { valor: 6,  palo: 'diamantes', letra: 'd' },
  { valor: 7,  palo: 'tréboles',  letra: 's' },
  { valor: 8,  palo: 'picas',     letra: 'f' },
  { valor: 9,  palo: 'corazones', letra: 'p' },
  { valor: 10, palo: 'diamantes', letra: 'c' },
  { valor: 11, palo: 'tréboles',  letra: 'b' },
  { valor: 12, palo: 'picas',     letra: 'g' },
  { valor: 13, palo: 'corazones', letra: 'z' },
])

// ── Estado reactivo ──
const indiceActual = ref(0)
const mostrarModal = ref(false)

// ── Derivados ──
const cartaActual = computed(() => mazo.value[indiceActual.value] ?? null)

// ── Manejadores ──
function siguienteCarta() {
  if (indiceActual.value < mazo.value.length - 1) {
    indiceActual.value++
  } else {
    mostrarModal.value = true
  }
}

function reiniciarPartida() {
  indiceActual.value = 0
  mostrarModal.value = false
}
</script>

<style scoped>
.aplicacion {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f1f5f9;
}

.aplicacion__contenido {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  padding: 2rem;
}

/* ── Victoria ── */
.victoria {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.2rem;
}

.victoria__mensaje {
  font-size: 1.1rem;
  color: #1e293b;
  margin: 0;
}
</style>
