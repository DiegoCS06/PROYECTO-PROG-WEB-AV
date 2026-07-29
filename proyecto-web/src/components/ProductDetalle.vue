<script setup>
const props = defineProps({
    product: {
        type: Object,
        required: true,
    },
})

const emit = defineEmits(['close'])
</script>

<template>
    <div class="detail-overlay" @click.self="emit('close')">
        <div class="detail-modal">
            <button class="detail-close" type="button" @click="emit('close')" aria-label="Cerrar">
                ✕
            </button>

            <div class="detail-grid">
                <img class="detail-image" :src="product?.imagen" :alt="product?.nombre" />

                <div class="detail-content">
                    <p class="detail-eyebrow">Detalle del producto</p>
                    <h2>{{ product?.nombre || 'Nombre del producto' }}</h2>
                    <p class="detail-price">{{ product?.precio || 'Precio' }}</p>

                    <section class="detail-section">
                        <h3>Descripción</h3>
                        <p>{{ product?.descripcion || 'Descripción.' }}</p>
                    </section>

                    <section class="detail-section">
                        <h3>Categoría</h3>
                        <p>{{ product?.categoria?.join(' • ') || 'Categoría.' }}</p>
                    </section>

                    <section class="detail-section">
                        <h3>Tallas</h3>
                        <p>{{ product?.tallas?.join(', ') || 'Tallas.' }}</p>
                    </section>

                    <section class="detail-section cart-section">
                        <div class="cart-controls">
                            <input class="cart-quantity" type="number" min="1" step="1" value="1" required>
                            <button class="cart-button" type="button">
                                Añadir a carrito
                            </button>
                        </div>
                    </section>

                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.detail-overlay {
    position: fixed;
    inset: 0;
    background: rgba(11, 11, 9, 0.7);
    backdrop-filter: blur(8px);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1100;
    padding: 1.5rem;
}

.detail-modal {
    background: linear-gradient(145deg, #fdfbf7 0%, #f4efe7 100%);
    width: min(92vw, 900px);
    max-height: 88vh;
    overflow-y: auto;
    border-radius: 24px;
    padding: 2rem;
    position: relative;
    box-shadow: 0 24px 70px rgba(15, 15, 12, 0.25);
}

.detail-close {
    position: absolute;
    top: 1rem;
    right: 1rem;
    width: 2.5rem;
    height: 2.5rem;
    border: none;
    border-radius: 50%;
    background: rgba(113, 108, 89, 0.12);
    color: var(--ecora-black, #0B0B09);
    cursor: pointer;
}

.detail-grid {
    display: grid;
    grid-template-columns: minmax(220px, 320px) 1fr;
    gap: 1.5rem;
    align-items: start;
}

.detail-image {
    width: 100%;
    aspect-ratio: 4 / 5;
    object-fit: cover;
    border-radius: 18px;
}

.detail-content {
    display: flex;
    flex-direction: column;
    gap: 0.9rem;
}

.detail-eyebrow {
    font-family: 'Jost', sans-serif;
    font-size: 0.76rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--ecora-stone, #716C59);
}

.detail-content h2 {
    font-family: 'Cormorant Garamond', serif;
    font-size: clamp(1.6rem, 2.4vw, 2.2rem);
    color: var(--ecora-black, #0B0B09);
}

.detail-price {
    font-family: 'Jost', sans-serif;
    font-size: 1rem;
    font-weight: 600;
    color: var(--ecora-warm, #6F5F48);
}

.detail-section {
    padding-top: 0.4rem;
    border-top: 1px solid rgba(113, 108, 89, 0.18);
}

.detail-section h3 {
    font-family: 'Jost', sans-serif;
    font-size: 0.95rem;
    margin-bottom: 0.3rem;
    color: var(--ecora-black, #0B0B09);
}

.detail-section p {
    font-family: 'Jost', sans-serif;
    line-height: 1.6;
    color: var(--ecora-stone, #716C59);
}

.cart-section {
    padding-top: 0.8rem;
}

.cart-controls {
    display: flex;
    gap: 0.75rem;
    align-items: center;
    flex-wrap: wrap;
}

.cart-quantity {
    width: 4.5rem;
    padding: 0.7rem 0.8rem;
    border: 1px solid rgba(113, 108, 89, 0.25);
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.85);
    color: var(--ecora-black, #0B0B09);
    font-family: 'Jost', sans-serif;
    font-size: 0.95rem;
    outline: none;
}

.cart-quantity:focus {
    border-color: var(--ecora-warm, #6F5F48);
    box-shadow: 0 0 0 3px rgba(111, 95, 72, 0.12);
}

.cart-button {
    border: none;
    border-radius: 999px;
    padding: 0.75rem 1rem;
    background: var(--ecora-black, #0B0B09);
    color: var(--ecora-cream, #F5F3EE);
    font-family: 'Jost', sans-serif;
    font-weight: 500;
    cursor: pointer;
    transition: transform 0.2s ease, background 0.2s ease;
}

.cart-button:hover {
    transform: translateY(-1px);
    background: var(--ecora-warm, #6F5F48);
}

@media (max-width: 720px) {
    .detail-grid {
        grid-template-columns: 1fr;
    }

    .detail-modal {
        padding: 1.5rem;
    }
}
</style>