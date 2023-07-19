<script setup>
    import {ref} from 'vue'
    // creo un state 
    // Este estate no es global es solo de este componente
    const numero = ref(0)

    // definino los props que recibira el componente
    const props = defineProps({
        guitarra: {
            type: Object,
            required: true
        }
    })

    defineEmits(['agregar-carrito'])

    
</script>

<template >
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
          <img 
            class="img-fluid" 
            :src="'/img/'+guitarra.imagen+'.jpg'"
            :alt="'imagen guitarra' + guitarra.nombre">
        </div>
        <div class="col-8">
          <h3 class="text-black fs-4 fw-bold text-uppercase">{{guitarra.nombre}}</h3>
          <p>{{numero}}</p>
          <p>{{guitarra.descripcion}}</p>
          <p class="fw-black text-primary fs-3">${{ guitarra.precio }}</p>
          <!-- 
            v-on:click= "numero++" agregera uno al state ref 
            v-on:click= "$emit('incrementar')" ejecuta la funcion incrementar de App.vue
        
            -->
          <button 
            type="button" 
            class="btn btn-dark w-100 "
            v-on:click= "$emit('agregar-carrito' ,guitarra)"
            >Agregar al carrito
           </button>
        </div>
    </div><!-- FIN GUITARRA -->
</template>

<style lang="">
    
</style>