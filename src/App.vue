<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefa: [
    
  ],
});
const getTarefasPendentes = () => {
  return estado.tarefa.filter(tarefa => !tarefa.concluida);
};

const getTarefasConcluidas = () => {
  return estado.tarefa.filter(tarefa => tarefa.concluida);
};

const getTarefasFiltradas = () => {
  const { filtro } = estado;
  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'concluidas':
      return getTarefasConcluidas();
    default:
      return estado.tarefa;
  }
};

const cadastraTarefa = () => {
  const tarefaNova = {
    nome: estado.tarefaTemp,
    concluida: false,
  }
  estado.tarefa.push(tarefaNova);
  estado.tarefaTemp = '';
};

const limparTarefas = () => {
  estado.tarefa = [];
};
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" class="form-control" placeholder="Digite sua tarefa">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Adicionar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="concluidas">Concluídas</option>
          </select>
        </div>
        <div class="col-md-2">
          <button @click="limparTarefas" type="button" class="btn btn-danger">Limpar Tudo</button>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()" :key="tarefa.nome">
        <input :checked="tarefa.concluida" :id="tarefa.nome" type="checkbox" @change="tarefa.concluida = !tarefa.concluida">
        <label :class="{ 'done': tarefa.concluida }" class="ms-3" :for="tarefa.nome">
          {{tarefa.nome}}
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
