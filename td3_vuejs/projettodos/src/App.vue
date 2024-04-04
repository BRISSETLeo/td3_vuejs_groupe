<template>
  <div>
    <h1>Liste des questionnaires</h1>
    <button @click="showQuestionnaireForm">Créer un questionnaire</button>
    <questionnaire-form v-if="showForm" @annulerCreateQuestionnaire="annulerCreateQuestionnaire" @createQuestionnaire="createQuestionnaire"></questionnaire-form>
    <ul>
      <li v-for="questionnaire in questionnaires" :key="questionnaire.id">
        {{ questionnaire.name }}
        <button @click="showQuestionnaireDetail(questionnaire)">Afficher les détails</button>
        <button @click="deleteQuestionnaire(questionnaire.id)">Supprimer</button>
        <button @click="ajouterQuestion(questionnaire.id)">Ajouter une question</button>
      </li>
    </ul>
    <div class="detail">
      <question v-if="showQuestion" v-for="question in questions" :key="question.id" :question="question" @editQuestion="modifierQuestion" @deleteQuestion="supprimerQuestion"></question>
    </div>
    
    <question-form v-if="showQuestionForm" :questionnaireId="selectedQuestionnaireId" @annulerCreateQuestion="annulerCreateQuestion" @createQuestion="createQuestion"></question-form>
  </div>
</template>

<script setup>
import QuestionnaireForm from './components/QuestionnaireForm.vue';
import QuestionForm from './components/QuestionForm.vue';
import Question from './views/Questions.vue'
import { ref } from 'vue';
import * as api from './Api.vue';

const questionnaires = ref([]);
const showForm = ref(false);
const showQuestionForm = ref(false);
const showQuestion = ref(false);
const detailContent = ref('');
const selectedQuestionnaireId = ref(null);
const questions = ref([]);
const questionnaire_id = ref(null);

async function fetchQuestionnaires() {
  try {
    const data = await api.recupererQuestionnaires();
    questionnaires.value = data.questionnaires.map(questionnaire => ({ id: questionnaire.id, name: questionnaire.name }));
  } catch (error) {
    console.error("Erreur lors de la récupération des questionnaires:", error);
  }
}

fetchQuestionnaires();

const createQuestionnaire = async (formData) => {
  try {
    await api.creerQuestionnaire(formData);
    fetchQuestionnaires();
    showForm.value = false;
  } catch (error) {
    console.error("Erreur lors de la création du questionnaire:", error);
  }
};

const annulerCreateQuestion = async () => {
  showQuestionForm.value = false;
};

const createQuestion = async (id_questionnaire, formData) => {
  try {
    await api.creerQuestion(id_questionnaire, formData);
    showQuestionForm.value = false;
  } catch (error) {
    console.error("Erreur lors de la création de la question:", error);
  }
};

const annulerCreateQuestionnaire = async () => {
  showForm.value = false;
};

const showQuestionnaireForm = () => {
  showForm.value = true;
};

const showQuestionnaireDetail = async (questionnaire) => {
  try {
    const data = await api.recupererQuestions(questionnaire.id);
    questions.value = data.questions;
    questionnaire_id.value = questionnaire.id;
    showQuestion.value = true;
  } catch (error) {
    console.error("Erreur lors de l'affichage des détails du questionnaire:", error);
  }
};

const deleteQuestionnaire = async (id) => {
  try {
    await api.supprimerQuestionnaire(id);
    fetchQuestionnaires();
    detailContent.value = '';
  } catch (error) {
    console.error("Erreur lors de la suppression du questionnaire:", error);
  }
};

const ajouterQuestion = async (questionnaireId) => {
  selectedQuestionnaireId.value = questionnaireId;
  showQuestionForm.value = true;
};

const modifierQuestion = async (id_question, formData) => {
  try {
    await api.modifierQuestion(id_question, formData);
  } catch (error) {
    console.error("Erreur lors de la modification de la question:", error);
  }
};

const supprimerQuestion = async(id) => {
  if(questionnaire_id == null) return;
  try {
    await api.supprimerQuestion(questionnaire_id.value, id);
    questions.value = questions.value.filter(q => q.id !== id);
  } catch (error) {
    console.error("Erreur lors de la suppression de la question:", error);
  }
};
</script>

<style scoped>
.detail {
  margin-top: 20px;
}
</style>