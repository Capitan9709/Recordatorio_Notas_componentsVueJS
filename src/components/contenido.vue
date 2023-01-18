<script setup>
    defineProps(['recordatorio']);

    const emit = defineEmits(['borrarNota','restarPendiente','cambiarPrioridad']);

    // funcion para enviar a app.vue el recordatorio a borrar
    function borrarNota(recordatorio){
        emit('borrarNota', recordatorio);
    }
    // funcion para enviar a app.vue la señal para terminar alli su funcion
    function restarPendiente(){
        emit('restarPendiente');
    }
    // funcion para marcar como hecho o no el recordatorio
    function marcarRecordatorio(recordatorio){
        recordatorio.hecho = !recordatorio.hecho;
        restarPendiente();
    }
    // funcion para cambiar la prioridad del recordatorio
    function prioridad(recordatorio, tipo){
        recordatorio.prioridad = tipo;
        emit('cambiarPrioridad');
    }
    // funcion para calcular los minutos que han pasado desde que se creo el recordatorio
    function calcularMinutos(recordatorio){
        var fechaActual = new Date();
        var fechaRecordatorio = new Date(recordatorio.fechaCreacion);
        var minutos = Math.round((fechaActual - fechaRecordatorio)/60000);
        return minutos;
    }
</script>

<template>      
    <div>      
        <i class='fa-regular ' v-bind:class='[{"fa-check-circle" : recordatorio.hecho}, {"fa-circle" : !recordatorio.hecho}]' id='hecho'  @click="marcarRecordatorio(recordatorio)"></i>
        <h2 v-bind:class='{"tachado" : recordatorio.hecho}'>{{ recordatorio.titulo }}</h2>
        <i class='fa-solid fa-ban' id='elimina' @click="borrarNota(recordatorio)"></i><br>
        <h6>Prioridad: 
            <button id='low' name='low' :class='[{selected : recordatorio.prioridad == 1}, {noselected : recordatorio.prioridad != 1}]' @click="prioridad(recordatorio, 1)">
                <i class='fa-solid fa-angle-down'></i>Low</button>
            <button id='normal' name='normal' :class='[{selected : recordatorio.prioridad == 2}, {noselected : recordatorio.prioridad != 2}]' @click="prioridad(recordatorio, 2)">Normal</button>
            <button id='high' name='high' :class='[{selected : recordatorio.prioridad == 3}, {noselected : recordatorio.prioridad != 3}]' @click="prioridad(recordatorio, 3)">
                <i class='fa-solid fa-angle-up'></i>High</button>
            ||  Añadido hace {{calcularMinutos(recordatorio)}} minutos</h6>
    </div>
</template>
<!-- 
<style scoped>
    
</style> -->