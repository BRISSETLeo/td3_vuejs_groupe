<template>
  <div class="container">
    <div class="left-pane">
      <h1>Liste des questionnaires</h1>
      <ul>
        <QuestionnaireItem v-for="questionnaire in questionnaires" :key="questionnaire.id"
          :questionnaire="questionnaire" @afficherDetail="afficherDetail" />
      </ul>
    </div>
    <div class="right-pane">
      <div class="detail" v-html="detailContent"></div>
    </div>
    <div class="center-pane">
      <div class="infoQuestion" v-html="infoContent"></div>
    </div>
  </div>
</template>

<script setup>
import QuestionnaireItem from './components/QuestionnaireItem.vue';
import { ref, onMounted } from 'vue';
import * as api from './Api.vue';

const questionnaires = ref([]);
const detailContent = ref('');
const infoContent = ref('');

onMounted(async () => {
  try {
    const data = await api.recupererQuestionnaires();
    console.log(data.questionnaires);
    questionnaires.value = data.questionnaires.map(questionnaire => ({ id: questionnaire.id, name: questionnaire.name }));
  } catch (error) {
    console.error("Erreur lors de la récupération des questionnaires:", error);
  }
});

const afficherDetail = async (idToDetail) => {
  try {
    const response = await fetch('http://127.0.0.1:5000/questionnaires/' + idToDetail.id + '/questions');
    const data = await response.json();
    var affichage = '';
    data.questions.forEach(question => {
      affichage += `<h2>${question.title}</h2>`;
      /*affichage += `<p>${question.question}</p>`;
      affichage += `<p>${question.question_type}</p>`;
      affichage += `<p>${question.questionnaire_id}</p>`;
      affichage += `<p>${question.id}</p>`;
      affichage += `<p>${question.answers ?? question.answer}</p>`;*/

    });
    console.log(data);
    detailContent.value = affichage;
  } catch (error) {
    console.error("Erreur lors de l'affichage des détails du questionnaire:", error);
  }
};
</script>

<style scoped>
.container {
  display: flex;
}

.left-pane {
  flex: 1;
  padding: 20px;
}

.right-pane {
  flex: 1;
  padding: 20px;
}

.infoQuestion {
  flex: 1;
  padding: 20px;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 10px;
}
</style>
