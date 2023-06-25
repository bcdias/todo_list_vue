<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaTarefas from './components/ListaTarefas.vue';

const estado = reactive({
  filtro: 'Todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: true
    },
    {
      titulo: 'Estudar SASS',
      finalizada: true
    },
    {
      titulo: 'Ir caminhar',
      finalizada: false
    },
  ]
})

const getTarefasPendente = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendente();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false
  }
  estado.tarefas.push(tarefaNova)
  estado.tarefaTemp = ''
}

</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendente().length"/>
    <Formulario :trocar-filtro="event => estado.filtro = event.target.value" :tarefa-temp="estado.tarefaTemp" :edita-taref-temp="event => estado.tarefaTemp = event.target.value" :cadastra-tarefa="cadastraTarefa" />
    <ListaTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>


