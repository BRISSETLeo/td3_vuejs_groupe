<template>
  <div>
    <h1>Liste des questionnaires</h1>
    <button @click="showQuestionnaireForm">Créer un questionnaire</button>
    <questionnaire-form v-if="showForm" @createQuestionnaire="createQuestionnaire"></questionnaire-form>
    <ul>
      <li v-for="questionnaire in questionnaires" :key="questionnaire.id">
        {{ questionnaire.name }}
        <button @click="showQuestionnaireDetail(questionnaire)">Afficher les détails</button>
        <button @click="deleteQuestionnaire(questionnaire.id)">Supprimer</button>
        <button @click="ajouterQuestion(questionnaire.id)">Ajouter une question</button>
      </li>
    </ul>
    <div class="detail" v-html="detailContent"></div>
    
    <question-form v-if="showQuestionForm" :questionnaireId="selectedQuestionnaireId" @createQuestion="createQuestion"></question-form>
  </div>
</template>

<script setup>
import QuestionnaireForm from './components/QuestionnaireForm.vue';
import QuestionForm from './components/QuestionForm.vue';
import { ref } from 'vue';
import * as api from './Api.vue';

const questionnaires = ref([]);
const showForm = ref(false);
const detailContent = ref('');
const showQuestionForm = ref(false);
const selectedQuestionnaireId = ref(null);

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

const createQuestion = async (id_questionnaire, formData) => {
  console.log(id_questionnaire, formData);
  try {
    await api.creerQuestion(id_questionnaire, formData);
    showQuestionForm.value = false;
  } catch (error) {
    console.error("Erreur lors de la création de la question:", error);
  }
};

const showQuestionnaireForm = () => {
  showForm.value = true;
};

const showQuestionnaireDetail = async (questionnaire) => {
  try {
    const data = await api.recupererQuestions(questionnaire.id);
    var affichage = '';
    data.questions.forEach(question => {
      affichage += `<div><h2>${question.title}</h2>
                    <button @click="modifierQuestion(question.id)">Modifier</button>
                    <button @click="supprimerQuestion(question.id)">Supprimer</button>
                    </div>`;
    });
    detailContent.value = affichage;
  } catch (error) {
    console.error("Erreur lors de l'affichage des détails du questionnaire:", error);
  }
};

const deleteQuestionnaire = async (id) => {
  try {
    await api.supprimerQuestionnaire(id);
    fetchQuestionnaires();
  } catch (error) {
    console.error("Erreur lors de la suppression du questionnaire:", error);
  }
};

const modifierQuestion = (questionId) => {
  // Implémentez la logique pour modifier une question ici
};

const supprimerQuestion = (questionId) => {
  // Implémentez la logique pour supprimer une question ici
};

const ajouterQuestion = async (questionnaireId) => {
  selectedQuestionnaireId.value = questionnaireId;
  showQuestionForm.value = true;
};

const editQuestion = async (id_questionnaire, formData) => {
  console.log(id_questionnaire, formData);
  try {
    await api.modifierQuestion(id_questionnaire, formData);
    showQuestionForm.value = false;
  } catch (error) {
    console.error("Erreur lors de la modification de la question:", error);
  }
};

</script>

<style scoped>
.detail {
  margin-top: 20px;
}
</style>
