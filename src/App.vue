<script setup>
  import cabecera from './components/cabecera.vue'
  import contenido from './components/contenido.vue'
  import pie from './components/pie.vue'

  import {ref} from 'vue';

  const tareasCompletadas = ref(0);
  const tareasTotales = ref(0);

  var campoFiltro = ref('');

  var recordatorios = ref([]);

  // carga los datos del local storage si existen
  if (localStorage.getItem('recordatorios')) {
    recordatorios.value = JSON.parse(localStorage.getItem('recordatorios'));
    console.log("Cargando del local storage");
    tareasTotales.value = recordatorios.value.length;
    restarPendiente();
  }
  
  // Introducir una nueva nota
  function introducirnotas(titulo) {
    recordatorios.value.push({
          codigoNota: recordatorios.value.length + 1,
          titulo: titulo,
          prioridad: 0,
          hecho: false,
          fechaCreacion: new Date()
      });
    tareasTotales.value = recordatorios.value.length;
    restarPendiente();
    guardarDatos();
  }
  // Borrar la nota deseada
  function borrarNota(recordatorio){
    let index = recordatorios.value.indexOf(recordatorio);
    recordatorios.value.splice(index,1);
    tareasTotales.value = recordatorios.value.length;
    restarPendiente();
    guardarDatos();
  }
  // Borrar todas las notas completadas
  function borrarCompletadas(){
    recordatorios.value = recordatorios.value.filter(function(recordatorio){
      return !recordatorio.hecho;
    });
    tareasTotales.value = recordatorios.value.length;
    restarPendiente();
    guardarDatos();
  }
  // Calcular las tareas pendientes
  function restarPendiente(){
    tareasCompletadas.value = recordatorios.value.filter((nota)=>nota.hecho == false).length;
    guardarDatos();
  }
  // Ordenar las notas por prioridad
  function prioridad(){
    recordatorios.value = recordatorios.value.sort(function(a,b){
        return a.prioridad - b.prioridad;            
    });
    guardarDatos();
  }
  // Filtrar las notas por palabras
  function filtrarContenido(){
    let miArrayFiltrado =  recordatorios.value.filter( (nota)=> nota.titulo.toLowerCase().includes(campoFiltro.value.toLowerCase()));
    return prioridad(miArrayFiltrado);
  }
  // Guardar en el locla storage
  function guardarDatos(){
    localStorage.setItem('recordatorios', JSON.stringify(recordatorios.value));
  }

</script>

<template>
  <cabecera @titulonota="introducirnotas"></cabecera>
  <hr>
  <p><i class="fa-solid fa-chart-column"></i> <span id="tareas-pendientes">{{ tareasCompletadas }}</span> Tareas pendientes de un total de <span id="tareas-totales">{{ tareasTotales }}</span> | <a id="borrarTodo" @click="borrarCompletadas"><i class="fa-solid fa-xmark"></i> Borrar tareas completadas</a><input type="text" id="filtro" placeholder="Filtra por palabras" v-model="campoFiltro" @keyup="filtrarContenido"></p>
  <hr>
  <div id="tareas" >
    <contenido id="tarea" v-for="recordatorio in recordatorios" @cambiarPrioridad="prioridad" @restarPendiente="restarPendiente" @borrarTareasCompletadas="borrarCompletadas" @borrarNota="borrarNota(recordatorio)" :recordatorio="recordatorio" :key="recordatorio.codigoNota">
      

    </contenido>
  </div>

  <pie></pie>
</template>

<style scoped>
  
</style>
