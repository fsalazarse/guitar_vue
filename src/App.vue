<script setup>
  import { ref, reactive, onMounted, watch} from 'vue'
  import { db } from './data/guitarras'
  import Guitarra from './components/Guitarra.vue'
  import Headers from './components/headers.vue'
  import Footer from './components/Footer.vue'

  //states
  const guitarras = ref([])
  const carrito = ref([])
  const guitarra  = ref({})

  // watch estara en todo momento viendo si algo cambia
  watch(carrito, () => {
    guardarLocalStorage()
  }, {
    deep: true
  })

  onMounted(() => {
    guitarras.value = db;
    guitarra.value = db[3];


    const carritoStorage = localStorage.getItem('carrito')
    if(carritoStorage) {
      carrito.value = JSON.parse(carritoStorage)
    }

  })

  const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
  }


  //custom event
  const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
    if (existeCarrito >= 0) {
      carrito.value[existeCarrito].cantidad++
    } else {
      guitarra.cantidad = 1;
      carrito.value.push(guitarra);
    }
  
  }

  const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if (carrito.value[index].cantidad <= 1) return  
    carrito.value[index].cantidad--
  }

  const aumentarrCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if (carrito.value[index].cantidad >= 5) return  
    carrito.value[index].cantidad++
  }

  const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id);
    
  }

  const limpiarCarrito = () => {
    carrito.value = []
  }


</script>

<template>
  <Headers 
   :carrito = "carrito"
   :guitarra = "guitarra"
   @decrementar-cantidad = "decrementarCantidad"
   @aumentar-cantidad = "aumentarrCantidad"
   @agregar-carrito = "agregarCarrito"
   @eliminar-producto = "eliminarProducto"
   @limpiar-carrito = "limpiarCarrito"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <!-- inicio el componente de Guitarra.vue 
        v-for: es una directiva  para recorrer la informacion del state. 
        v-bind: es un Props(permite la comunicacion dentre los componetes) el objeto iterado guitarra pra poder utilizarlo en el componente hijo  
        @incrementar = "incrementar" defino que se ejecutar esta funcion cuando Guitarra.vue ejecute un evento
      -->

      <Guitarra 
        v-for="guitarra in guitarras"
        :key="guitarra.id"
        v-bind:guitarra = "guitarra"
        @agregar-carrito = "agregarCarrito"
      />

    </div>
  </main>

  <Footer />

  
</template>
