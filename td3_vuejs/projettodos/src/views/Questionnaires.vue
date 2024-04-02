<template>
  <div>
    <h1>Liste des questionnaires</h1>
    <button @click="showQuestionnaireForm">Créer un questionnaire</button>
    <questionnaire-form v-if="showForm" @createQuestionnaire="createQuestionnaire"></questionnaire-form>
    <ul>
      <li v-for="questionnaire in questionnaires" :key="questionnaire.id">
        {{ questionnaire.title }}
        <button @click="editQuestionnaire(questionnaire)">Modifier</button>
        <button @click="deleteQuestionnaire(questionnaire.id)">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<script>
import QuestionnaireForm from '@/components/QuestionnaireForm.vue';

export default {
  components: {
    QuestionnaireForm
  },
  data() {
    return {
      questionnaires: [],
      showForm: false,
      editedQuestionnaire: null
    }
  },
  methods: {
    showQuestionnaireForm() {
      this.showForm = true;
    },
    createQuestionnaire(formData) {
      // Ajouter la logique pour créer un questionnaire
      this.questionnaires.push({
        id: Math.random().toString(36).substr(2, 9),
        title: formData.title
      });
      this.showForm = false;
    },
    editQuestionnaire(questionnaire) {
      this.showForm = true;
      this.editedQuestionnaire = questionnaire;
    },
    deleteQuestionnaire(id) {
      // Ajouter la logique pour supprimer un questionnaire
      this.questionnaires = this.questionnaires.filter(q => q.id !== id);
    }
  }
}
</script>
