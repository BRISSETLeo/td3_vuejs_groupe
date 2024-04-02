<template>
  <div>
    <h1>Liste des questions</h1>
    <button @click="showQuestionForm">Créer une question</button>
    <question-form v-if="showForm" @createQuestion="createQuestion"></question-form>
    <ul>
      <li v-for="question in questions" :key="question.id">
        {{ question.content }}
        <button @click="editQuestion(question)">Modifier</button>
        <button @click="deleteQuestion(question.id)">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<script>
import QuestionForm from '@/components/QuestionForm.vue';

export default {
  components: {
    QuestionForm
  },
  data() {
    return {
      questions: [],
      showForm: false,
      editedQuestion: null
    }
  },
  methods: {
    showQuestionForm() {
      this.showForm = true;
    },
    createQuestion(formData) {
      // Ajouter la logique pour créer une question
      this.questions.push({
        id: Math.random().toString(36).substr(2, 9),
        content: formData.content
      });
      this.showForm = false;
    },
    editQuestion(question) {
      this.showForm = true;
      this.editedQuestion = question;
    },
    deleteQuestion(id) {
      // Ajouter la logique pour supprimer une question
      this.questions = this.questions.filter(q => q.id !== id);
    }
  }
}
</script>
