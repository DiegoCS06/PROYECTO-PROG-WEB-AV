<script setup>
import { ref, onMounted, computed, nextTick } from 'vue';
import AppHeader from './components/AppHeader.vue'
import HeroSection from './components/HeroSection.vue'
import CollectionSection from './components/CollectionSection.vue';
import NosotrosSection from './components/NosotrosSection.vue'
import ProcesoSection from './components/ProcesoSection.vue';
import ValoresSection from './components/ValoresSection.vue';
import TeamSection from './components/TeamSection.vue';
import ContactoSection from './components/ContactoSection.vue';
import AppFooter from './components/AppFooter.vue';
import CollectionModal from './components/CollectionModal.vue';
import RegisterModal from './components/RegisterModal.vue'
import LoginModal from './components/LoginModal.vue'
import CartSide from './components/CartSide.vue'
import AdminPanel from './components/AdminPanel.vue'

const data = ref([])
const error = ref('')
const isAdminPanelOpen = ref(false)
const productos = ref([])
const ordenes = ref([])

onMounted(async () => {
  crearAdministradorInicial()

  try {
    const response = await fetch('/data/Catalogo.json')

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`)
    }

    data.value = await response.json()

    const productosGuardados =
      localStorage.getItem('ecoraProductos')

    if (productosGuardados) {
      try {
        productos.value = JSON.parse(productosGuardados)
      } catch (errorProductos) {
        console.error(
          'No fue posible recuperar los productos guardados:',
          errorProductos
        )

        localStorage.removeItem('ecoraProductos')

        productos.value = data.value.catalogo.map((producto) => ({
          ...producto,
          inventario: producto.inventario ?? 10
        }))

        guardarProductos()
      }
    } else {
      productos.value = data.value.catalogo.map((producto) => ({
        ...producto,
        inventario: producto.inventario ?? 10
      }))

      guardarProductos()
    }
  } catch (err) {
    error.value = 'No Products found! :('
    console.error(err)
  }

  const sesionGuardada = localStorage.getItem('ecoraSesion')

  if (sesionGuardada) {
    try {
      usuarioSesion.value = JSON.parse(sesionGuardada)
    } catch (errorSesion) {
      console.error(
        'No fue posible recuperar la sesión:',
        errorSesion
      )

      localStorage.removeItem('ecoraSesion')
      usuarioSesion.value = null
    }
  }

  const ordenesGuardadas = localStorage.getItem('ecoraOrdenes')

  if (ordenesGuardadas) {
    try {
      ordenes.value = JSON.parse(ordenesGuardadas)
    } catch (errorOrdenes) {
      console.error(
        'No fue posible recuperar las órdenes:',
        errorOrdenes
      )

      localStorage.removeItem('ecoraOrdenes')
      ordenes.value = []
    }
  }
})

const isModalOpen = ref(false)
const isRegisterOpen = ref(false)

const manejarRegistroExitoso = (nuevoUsuario) => {
  console.log('Usuario registrado:', nuevoUsuario)
}

const isLoginOpen = ref(false)
const usuarioSesion = ref(null)

const manejarInicioSesion = (usuario) => {
  usuarioSesion.value = usuario
  isLoginOpen.value = false
}

const cerrarSesion = () => {
  localStorage.removeItem('ecoraSesion')
  usuarioSesion.value = null
  isAdminPanelOpen.value = false
}

// ── Estado del carrito ──
const cartOpen = ref(false)
const cartItems = ref([])

const addToCart = (item) => {
  // Buscar si ya existe el mismo producto con la misma talla
  const existing = cartItems.value.find(
    i => i.id === item.id && i.talla === item.talla
  )

  if (existing) {
    existing.cantidad += item.cantidad
  } else {
    cartItems.value.push({ ...item })
  }
}

const removeFromCart = (index) => {
  cartItems.value.splice(index, 1)
}

const cartCount = computed(() =>
  cartItems.value.reduce(
    (sum, i) => sum + Number(i.cantidad || 0),
    0
  )
)

const obtenerPrecioNumerico = (precio) => {
  if (typeof precio === 'number') {
    return precio
  }

  if (typeof precio === 'string') {
    return Number(precio.replace(/\D/g, '')) || 0
  }

  return 0
}

const cartTotal = computed(() =>
  cartItems.value.reduce((sum, i) => {
    const precio = obtenerPrecioNumerico(i.precio)

    return sum + precio * Number(i.cantidad || 0)
  }, 0)
)

const clearCart = () => {
  cartItems.value = []
}

const filtroColeccion = ref('Todos')

const aplicarFiltro = async (tipo) => {
  filtroColeccion.value = tipo
  await nextTick()

  document
    .querySelector('#coleccion')
    ?.scrollIntoView({ behavior: 'smooth' })
}

const crearAdministradorInicial = () => {
  let usuariosGuardados = []

  try {
    usuariosGuardados =
      JSON.parse(localStorage.getItem('ecoraUsuarios')) || []
  } catch (errorUsuarios) {
    console.error(
      'No fue posible recuperar los usuarios:',
      errorUsuarios
    )

    localStorage.removeItem('ecoraUsuarios')
  }

  const correoAdministrador = 'admin@ecora.com'

  const indiceAdministrador = usuariosGuardados.findIndex(
    (usuario) =>
      usuario.correo?.toLowerCase() === correoAdministrador
  )

  if (indiceAdministrador !== -1) {
    usuariosGuardados[indiceAdministrador] = {
      ...usuariosGuardados[indiceAdministrador],
      nombre: 'Administrador Ecora',
      correo: correoAdministrador,
      contrasenna: 'Admin123',
      rol: 'administrador'
    }
  } else {
    const administrador = {
      id: Date.now(),
      nombre: 'Administrador Ecora',
      correo: correoAdministrador,
      contrasenna: 'Admin123',
      rol: 'administrador'
    }

    usuariosGuardados.push(administrador)
  }

  localStorage.setItem(
    'ecoraUsuarios',
    JSON.stringify(usuariosGuardados)
  )
}

const abrirPanelAdministrador = () => {
  if (usuarioSesion.value?.rol !== 'administrador') {
    return
  }

  isAdminPanelOpen.value = true
}

const guardarProductos = () => {
  localStorage.setItem(
    'ecoraProductos',
    JSON.stringify(productos.value)
  )
}

const registrarProducto = (nuevoProducto) => {
  const idsValidos = productos.value
    .map((producto) => Number(producto.id))
    .filter((id) => Number.isFinite(id))

  const nuevoId = idsValidos.length
    ? Math.max(...idsValidos) + 1
    : 1

  productos.value.push({
    ...nuevoProducto,
    id: nuevoId
  })

  guardarProductos()
}

const editarProducto = (productoEditado) => {
  const indice = productos.value.findIndex(
    (producto) =>
      Number(producto.id) === Number(productoEditado.id)
  )

  if (indice === -1) {
    return
  }

  productos.value[indice] = {
    ...productos.value[indice],
    ...productoEditado
  }

  guardarProductos()
}

const eliminarProducto = (productoId) => {
  productos.value = productos.value.filter(
    (producto) =>
      Number(producto.id) !== Number(productoId)
  )

  guardarProductos()
}

// ── Estado de órdenes ──
const guardarOrdenes = () => {
  localStorage.setItem(
    'ecoraOrdenes',
    JSON.stringify(ordenes.value)
  )
}

const registrarOrden = (nuevaOrden) => {
  const idsValidos = ordenes.value
    .map((orden) => Number(orden.id))
    .filter((id) => Number.isFinite(id))

  const nuevoId = idsValidos.length
    ? Math.max(...idsValidos) + 1
    : 1

  ordenes.value.push({
    ...nuevaOrden,
    id: nuevoId,
    estado: nuevaOrden.estado || 'Pendiente'
  })

  guardarOrdenes()
  clearCart()
  cartOpen.value = false
}

const actualizarEstadoOrden = ({ id, estado }) => {
  const indice = ordenes.value.findIndex(
    (orden) => Number(orden.id) === Number(id)
  )

  if (indice === -1) {
    return
  }

  ordenes.value[indice] = {
    ...ordenes.value[indice],
    estado
  }

  guardarOrdenes()
}
</script>

<template>
  <AppHeader
    :usuario="usuarioSesion"
    :cart-count="cartCount"
    @open-register="isRegisterOpen = true"
    @open-login="isLoginOpen = true"
    @open-admin="abrirPanelAdministrador"
    @logout="cerrarSesion"
    @open-cart="cartOpen = true"
  />

  <HeroSection
    @open-collection="isModalOpen = true"
  />

  <CollectionSection
    :catalogo="productos"
    :filtro="filtroColeccion"
    @add-to-cart="addToCart"
  />

  <NosotrosSection />
  <ValoresSection />
  <TeamSection />
  <ProcesoSection />
  <ContactoSection />

  <AppFooter
    @filtrar-coleccion="aplicarFiltro"
  />

  <CollectionModal
    v-if="isModalOpen"
    :catalogo="productos"
    :filtro-inicial="filtroColeccion"
    @close="isModalOpen = false"
    @add-to-cart="addToCart"
  />

  <CartSide
    v-if="cartOpen"
    :items="cartItems"
    :total="cartTotal"
    :usuario="usuarioSesion"
    @close="cartOpen = false"
    @remove-item="removeFromCart"
    @cart-paid="registrarOrden"
  />

  <RegisterModal
    v-if="isRegisterOpen"
    @close="isRegisterOpen = false"
    @registered="manejarRegistroExitoso"
  />

  <LoginModal
    v-if="isLoginOpen"
    @close="isLoginOpen = false"
    @login-success="manejarInicioSesion"
  />

  <AdminPanel
    v-if="
      isAdminPanelOpen &&
      usuarioSesion?.rol === 'administrador'
    "
    :productos="productos"
    :ordenes="ordenes"
    @close="isAdminPanelOpen = false"
    @registrar-producto="registrarProducto"
    @editar-producto="editarProducto"
    @eliminar-producto="eliminarProducto"
    @actualizar-estado-orden="actualizarEstadoOrden"
  />
</template>

<style>
/* Reset global y variables de Ecora */
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

:root {
  --ecora-cream: #F5F3EE;
  --ecora-stone: #716C59;
  --ecora-sand: #AEA897;
  --ecora-black: #0B0B09;
  --ecora-warm: #6F5F48;
  --ecora-gray: #A5A8AF;
  --ecora-error: #c0392b;
}

body {
  background: var(--ecora-cream);
  color: var(--ecora-black);
  font-family: 'Jost', sans-serif;
  -webkit-font-smoothing: antialiased;
}

img {
  max-width: 100%;
  display: block;
}

a {
  text-decoration: none;
  color: inherit;
}

.form-input,
.form-group {
  min-width: 0;
  box-sizing: border-box;
}
</style>
