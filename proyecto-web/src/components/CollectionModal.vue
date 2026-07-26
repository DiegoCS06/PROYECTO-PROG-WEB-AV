<script setup>
const emit = defineEmits(['close'])

const props = defineProps({
  catalogo: {
    type: Array,
    required: true,
  },
})
</script>

<template>
  <div class="modal-overlay" @click.self="emit('close')">
    <div class="modal-content">
      <button class="modal-close" type="button" @click="emit('close')" aria-label="Cerrar">
        ✕
      </button>

      <div style="display: flex; flex-direction: column;" class="modal-header">
        <p class="modal-eyebrow">Colección Ecora</p>
        <p class="modal-description">   
          Explora piezas únicas creadas con materiales reciclados y un diseño atemporal.
        </p>
      </div>

      <div v-if="catalogo?.length" class="items-grid">
        <article v-for="producto in catalogo" :key="producto.nombre" class="item-card">
          <img class="product-image" :src="producto.imagen" :alt="producto.nombre">
          <div class="item-body">
            <h2>{{ producto.nombre }}</h2>
            <p class="item-price">{{ producto.precio }}</p>
            <button class="detail-button" type="button">
              Ver detalles
            </button>
          </div>
        </article>
      </div>

      <div v-else class="empty-state">
        <p>No hay productos disponibles por el momento.</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@500;600&family=Jost:wght@300;400;500&display=swap');

.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(11, 11, 9, 0.68);
  backdrop-filter: blur(8px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 1.5rem;
}

.modal-content {
  background: linear-gradient(145deg, #fdfbf7 0%, #f4efe7 100%);
  padding: 2.25rem;
  border-radius: 24px;
  width: min(92vw, 920px);
  max-height: 86vh;
  overflow-y: auto;
  position: relative;
  box-shadow: 0 24px 70px rgba(15, 15, 12, 0.25);
  border: 1px solid rgba(113, 108, 89, 0.16);
}

.modal-close {
  position: absolute;
  top: 1rem;
  right: 1rem;
  width: 2.5rem;
  height: 2.5rem;
  border: none;
  border-radius: 50%;
  display: grid;
  place-items: center;
  background: rgba(113, 108, 89, 0.12);
  color: var(--ecora-black, #0B0B09);
  font-size: 1.1rem;
  cursor: pointer;
  transition: transform 0.2s ease, background 0.2s ease;
}

.modal-close:hover {
  transform: rotate(90deg);
  background: rgba(113, 108, 89, 0.2);
}

.modal-header {
  margin-bottom: 1.75rem;
  padding-right: 3rem;
}

.modal-eyebrow {
  font-family: 'Jost', sans-serif;
  font-size: 0.76rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--ecora-stone, #716C59);
  margin-bottom: 0.6rem;
}

.modal-header h1 {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(2rem, 3.2vw, 2.8rem);
  font-weight: 600;
  color: var(--ecora-black, #0B0B09);
  margin-bottom: 0.55rem;
}

.modal-description {
  font-family: 'Jost', sans-serif;
  color: var(--ecora-stone, #716C59);
  line-height: 1.7;
  max-width: 640px;
}

.items-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}

.item-card {
  background: rgba(255, 255, 255, 0.74);
  border: 1px solid rgba(113, 108, 89, 0.12);
  border-radius: 18px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 24px rgba(11, 11, 9, 0.05);
}

.product-image {
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
}

.item-body {
  padding: 1.1rem 1.1rem 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
}

.item-body h2 {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.2rem;
  color: var(--ecora-black, #0B0B09);
}

.item-price {
  font-family: 'Jost', sans-serif;
  font-size: 0.95rem;
  color: var(--ecora-stone, #716C59);
  font-weight: 500;
}

.detail-button {
  margin-top: auto;
  border: none;
  border-radius: 999px;
  padding: 0.7rem 1rem;
  background: var(--ecora-black, #0B0B09);
  color: var(--ecora-cream, #F5F3EE);
  font-family: 'Jost', sans-serif;
  font-weight: 500;
  cursor: pointer;
  transition: transform 0.2s ease, background 0.2s ease;
}

.detail-button:hover {
  transform: translateY(-1px);
  background: var(--ecora-warm, #6F5F48);
}

.empty-state {
  padding: 2rem;
  border: 1px dashed rgba(113, 108, 89, 0.3);
  border-radius: 16px;
  text-align: center;
  color: var(--ecora-stone, #716C59);
  font-family: 'Jost', sans-serif;
}

@media (max-width: 640px) {
  .modal-content {
    padding: 1.5rem;
  }

  .modal-header {
    padding-right: 2.2rem;
  }

  .items-grid {
    grid-template-columns: 1fr;
  }
}
</style>