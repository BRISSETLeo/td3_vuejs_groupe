<template>
  <div>
    <h1>Liste des tâches</h1>
    <ul>
      <TodoItem v-for="task in tasks" :key="task.id" :task="task" @remove="removeItem" @modifier="modifierItem"/>
    </ul>
  </div>
</template>

<script setup>
import TodoItem from './components/TodoItem.vue';
import { ref, onMounted } from 'vue';

const tasks = ref([]);

onMounted(async () => {
  const response = await fetch('http://localhost:5000/todo/api/v1.0/tasks');
  const data = await response.json();
  tasks.value = data.tasks.map(task => ({ id: task.id, title: task.description }));
});

const removeItem = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id.id);
};

const modifierItem = (id) => {
  for (let i = 0; i < tasks.value.length; i++) {
    if (tasks.value[i].id === id.id) {
      tasks.value[i].title = prompt('Entrez le nouveau titre de la tâche');
      break;
    }
  }
};
</script>

<style scoped>
.app {
  text-align: center;
}
</style>
