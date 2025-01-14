<script setup>
import { reactive } from 'vue';

  const estado = reactive({
    filtro: 'Todas',
    tarefaTemp: '',
    tarefas: [{
      titulo: 'Estudar ES6',
      finalizada: false,
    },{
      titulo: 'Estudar SASS',
      finalizada: false,
    }, {
      titulo: 'Ir a academia',
      finalizada: true
    }]
  });

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
  };

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
  };

  const getTarefasFiltradas = () => {
    const filtro = estado.filtro;

    switch(filtro) {
      case 'Pendentes':
        return getTarefasPendentes()
      case 'Finalizadas':
        return getTarefasFinalizadas()
      default:
        return estado.tarefas
    }
  };

  const cadastrarTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova)
    estado.tarefaTemp = '';
  };

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>{{ getTarefasPendentes().length }}</p>
    </header>
    <form @submit.prevent="cadastrarTarefa()">
      <div class="row">
        <div class="col-md-8">
        <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Casdastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="Todas">Todas as tarefas</option>
            <option value="Pendentes">Pendentes</option>
            <option value="Finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox" @change="evento => tarefa.finalizada = evento.target.checked">
        <label :class="{done: tarefa.finalizada}" class="ms-3" for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>

.done{
  text-decoration: line-through;
}
</style>
