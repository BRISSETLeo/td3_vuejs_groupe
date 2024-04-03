<template>
  <div>
    <ul>
      <li v-for="question in questions" :key="question.id">
        {{ question.title }}
        <button @click="editQuestion(question)">Modifier</button>
        <button @click="deleteQuestion(question.id)">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const questions = ref([]);
const showForm = ref(false);
const editedQuestion = ref(null);

const editQuestion = (question) => {
  showForm.value = true;
  editedQuestion.value = question;
};

const deleteQuestion = (id) => {
  questions.value = questions.value.filter(question => question.id !== id);
};
</script>
