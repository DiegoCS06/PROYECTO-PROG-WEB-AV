<template>
  <section class="nosotros" id="nosotros">
    <div class="nosotros__inner">

      <!-- COLUMNA IMAGEN -->
      <div class="nosotros__imagen-wrap">
        <img
          src="/nosotros.jpg"
          alt="Telas recicladas en taller de Ecora, fotografía tomada de @Greenlosophy en Pinterest"
          class="nosotros__imagen"
        >
        <div class="nosotros__badge">
          <span class="nosotros__badge-year">DESDE 2023</span>
          <p class="nosotros__badge-text">Moda que respeta el planeta</p>
        </div>
      </div>

      <!-- COLUMNA TEXTO -->
      <div class="nosotros__contenido">

        <span class="nosotros__eyebrow">Quiénes somos</span>

        <h2 class="nosotros__titulo">
          Una marca nacida<br>
          del <em>compromiso.</em>
        </h2>

        <p class="nosotros__descripcion">
          Ecora nació de la convicción de que la industria de la moda puede
          —y debe— cambiar. Creamos prendas únicas a partir de tela reciclada,
          dándole una segunda vida a materiales que de otro modo terminarían
          en vertederos.
        </p>

        <!-- MISIÓN Y VISIÓN -->
        <div class="nosotros__mv">
          <div class="nosotros__mv-item">
            <h3 class="nosotros__mv-titulo">Misión</h3>
            <p class="nosotros__mv-texto">
              Brindar una alternativa de moda sostenible con prendas ecológicas,
              promoviendo un consumo consciente sin sacrificar estilo ni calidad.
            </p>
          </div>
          <div class="nosotros__mv-item">
            <h3 class="nosotros__mv-titulo">Visión</h3>
            <p class="nosotros__mv-texto">
              Convertirnos en marca referente de moda sostenible, reconocida por
              demostrar que la ropa puede tener una segunda vida sin perder diseño
              ni personalidad.
            </p>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300;1,400&family=Jost:wght@300;400;500&display=swap');

.nosotros {
  background: #F5F3EE;
  padding: 7rem 2rem;
}

.nosotros__inner {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5rem;
  align-items: center;
}

/* ── Imagen ── */
.nosotros__imagen-wrap {
  position: relative;
}

.nosotros__imagen {
  width: 100%;
  height: 580px;
  object-fit: cover;
  display: block;
}

.nosotros__badge {
  position: absolute;
  bottom: 2rem;
  left: 2rem;
  background: #F5F3EE;
  padding: 1.25rem 1.5rem;
  max-width: 200px;
}

.nosotros__badge-year {
  font-family: 'Jost', sans-serif;
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: #716C59;
  display: block;
  margin-bottom: 0.5rem;
}

.nosotros__badge-text {
  font-family: 'Jost', sans-serif;
  font-size: 14px;
  font-weight: 400;
  color: #0B0B09;
  line-height: 1.4;
}

/* ── Contenido ── */
.nosotros__contenido {
  display: flex;
  flex-direction: column;
  gap: 1.75rem;
}

.nosotros__eyebrow {
  font-family: 'Jost', sans-serif;
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #716C59;
}

.nosotros__titulo {
  font-family: 'Cormorant Garamond', serif;
  font-size: clamp(2.5rem, 4vw, 3.5rem);
  font-weight: 400;
  line-height: 1.15;
  color: #0B0B09;
}

.nosotros__titulo em {
  font-style: italic;
  font-weight: 300;
}

.nosotros__descripcion {
  font-family: 'Jost', sans-serif;
  font-size: 15px;
  font-weight: 300;
  line-height: 1.75;
  color: #716C59;
  max-width: 480px;
}

/* ── Misión y Visión ── */
.nosotros__mv {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0;
  border-top: 1px solid #AEA897;
  padding-top: 1.75rem;
}

.nosotros__mv-item {
  padding-right: 2rem;
  border-left: 2px solid #AEA897;
  padding-left: 1.25rem;
}

.nosotros__mv-item:first-child {
  border-left: none;
  padding-left: 0;
  border-right: 1px solid #AEA897;
  padding-right: 2rem;
}

.nosotros__mv-titulo {
  font-family: 'Cormorant Garamond', serif;
  font-size: 18px;
  font-weight: 400;
  color: #0B0B09;
  margin-bottom: 0.75rem;
}

.nosotros__mv-texto {
  font-family: 'Jost', sans-serif;
  font-size: 13px;
  font-weight: 300;
  line-height: 1.7;
  color: #716C59;
}

/* ── Responsive ── */
@media (max-width: 900px) {
  .nosotros__inner {
    grid-template-columns: 1fr;
    gap: 3rem;
  }

  .nosotros__imagen {
    height: 420px;
  }

  .nosotros__titulo {
    font-size: 2.25rem;
  }
}

@media (max-width: 480px) {
  .nosotros {
    padding: 4rem 1.25rem;
  }

  .nosotros__mv {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .nosotros__mv-item:first-child {
    border-right: none;
    border-bottom: 1px solid #AEA897;
    padding-bottom: 1.5rem;
  }

  .nosotros__mv-item {
    padding-right: 0;
  }
}
</style>