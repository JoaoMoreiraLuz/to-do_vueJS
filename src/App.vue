<script setup>
    import { reactive } from 'vue';
    import Cabecalho from './components/cabecalho.vue';
    import Formulario from './components/formulario.vue';
    import ListaDeTarefas from './components/listaDeTarefas.vue';

const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
    ]
})

const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
        case 'pendentes':
            return getTarefasPendentes();
        case 'finalizadas':
            return getTarefasFinalizadas();
        default:
            return estado.tarefas;
    }
}
const cadastraTarefa = () => {
    const tarefaNova = {
        titulo: estado.tarefaTemp,
        finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
}
</script>

<template>
    <div class="container">
        <cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
        <formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" />
        <listaDeTarefas :tarefas="getTarefasFiltradas()" />
    </div>
</template>


