<script setup>
import { ref, onMounted, computed, watch } from 'vue';
import ToDoItem from './components/ToDoItem.vue';

const todos = ref([]);
const name = ref('');

const input_content = ref('');
const input_category = ref(null);
const input_priority = ref(null);

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt;
}));

watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
});

onMounted(() => {
  name.value = localStorage.getItem('name') || '';

  todos.value = JSON.parse(localStorage.getItem('todos') || []);
});

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null || input_priority.value === null) {
    return;
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    priority: input_priority.value,
    done: false,
    createdAt: new Date().getTime(),
  });
  input_content.value = '';
  input_category.value = null;
  input_priority.value = null;
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter(t => t !== todo);
};

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal));
}, { deep: true });
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Welcome,
        <input type="text" placeholder="Nome" v-model="name" />

      </h2>
    </section>

    <section class="create-todo">
      <form @submit.prevent="addTodo">
        <h4>Nova tarefa...</h4>

        <input type="text" placeholder="ex.: preencher to-do list" v-model="input_content">


        <div class="options">
        <div>
          <h4>Selecione uma categoria</h4>
          <select v-model="input_category">
            <option disabled value="null">Categoria</option>
            <option value="school">Estudos</option>
            <option value="shop">Compras</option>
            <option value="personal">Pessoal</option>
          </select>
        </div>
          <div>
            <h4>Selecione a prioridade</h4>
            <select v-model="input_priority">
            <option disabled value="null">Prioridade</option>
            <option value="high">Alta</option>
            <option value="medium">Média</option>
            <option value="low">Baixa</option>
          </select>
          </div>



        </div>

        <input type="submit" value="Adicionar">

      </form>

    </section>
    <section class="todo-list">
      <h3>Tarefas</h3>
      <div class="list">
        <ToDoItem
          v-for="todo in todos_asc"
          :key="todo.createdAt"
          :todo="todo"
          @remove="removeTodo"
        />
      </div>
    </section>
  </main>
</template>