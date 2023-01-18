<script setup>
    import {ref} from 'vue';
    var textoInput = ref('');

    const emit = defineEmits(['titulonota']);

    
    // funcion para enviar a app.vue el titulo del recordatorio a crear
    function submit() {
        emit('titulonota', textoInput.value);
        textoInput.value = '';
    }

</script>

<template>
    <h1>Proyecto Recordatorios - Luis Moya Prados</h1>

    <input type="text" id="texto" placeholder="¿Qué quieres recordar?" v-model="textoInput" @keyup.enter.prevent="submit" v-on:submit="submit">
    <input type="submit" id="boton" value="Añadir">
    
</template>

<style scoped>
    
</style>