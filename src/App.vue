<script setup>
import {onMounted, ref} from 'vue';
import axios from "axios"
import ExternalComponent from './components/ExternalComponent.vue';
const datos = ref([])
let pagina = Math.floor(Math.random() * 34 + 1)

onMounted(async () => {
   await getDatos();
   setTimeout(async() => {
    pagina = Math.floor(Math.random() * 34 + 1)
    await getDatos();
   }, 3000)
});

function hacerAlgo(){
    console.log("Se hizo click")
}


async function getDatos(){
    try {
        const datosTransformados = []
        const respuesta = await axios.get('https://rickandmortyapi.com/api/character/?page=' + pagina)
        //console.log(respuesta.data.results)
        respuesta.data.results.forEach(element=>{
            console.log(element)
            let elementoTemporal = {
                titulo: element.name,
                contenido: element.status,
                imagen: element.image,
            }
            datosTransformados.push(elementoTemporal)
        });
        console.log(datosTransformados)
        datos.value = datosTransformados
        console.log
    } catch (error) {
        console.log("No se pudo acceder a la libreria remota", error.message)
    }
}

</script>

<template>

<div class="container-expand">
    <button v-on:click="getDatos()">Consumir API</button>
    <div class="row">
        <div v-for="dato in datos"  class="col-3">
            <ExternalComponent :contenido="dato.contenido" :titulo="dato.titulo" :image="dato.imagen"/>
        </div>
    </div>
</div>

</template>

<style>

.parrafo{
    color: aqua;
    font-size: 24px;
}

</style>