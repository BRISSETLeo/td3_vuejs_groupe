<template>
  <div class="app">
    <h1>Liste des questionnaires</h1>
    <ul>
      <QuestionnaireItem 
        v-for="questionnaire in questionnaires" 
        :key="questionnaire.id" 
        :questionnaire="questionnaire" 
        @afficherDetail="afficherDetail"
      />
    </ul>
  </div>
  <div class="detail">
  </div>
</template>

<script setup>
import QuestionnaireItem from './components/QuestionnaireItem.vue';
import { ref, onMounted } from 'vue';
import * as api from './Api.vue';

const questionnaires = ref([]);
const detailContent = ref('');

onMounted(async () => {
  try {
    const data = await api.recupererQuestionnaires();
    console.log(data.questionnaires);
    questionnaires.value = data.questionnaires.map(questionnaire => ({ id: questionnaire.id, name: questionnaire.name }));
  } catch (error) {
    console.error("Erreur lors de la récupération des questionnaires:", error);
  }
});

const removeItem = (idToRemove) => {
  questionnaires.value = questionnaires.value.filter(questionnaire => questionnaire.id !== idToRemove);
};

const modifierItem = (idToModify) => {
  const questionnaireToModify = questionnaires.value.find(questionnaire => questionnaire.id === idToModify);
  if (questionnaireToModify) {
    const newName = prompt('Entrez le nouveau titre du questionnaire', questionnaireToModify.name);
    if (newName !== null) {
      questionnaireToModify.name = newName;
    }
  }
};

const afficherDetail = (idToDetail) => {
  const questionnaireToDisplayDetail = questionnaires.value.find(questionnaire => questionnaire.id === idToDetail.id);
  fetch('http://127.0.0.1:5000/questionnaires/'+idToDetail.id+'/questions')
  .then(response => response.json())
  .then(data => {
    var affichage = "<h3>"+idToDetail.id+" "+questionnaireToDisplayDetail.name+"</h3>";
    data.questions.forEach(question => {
      affichage += (question.answers ?? question.answer);
    });
    // Sélectionnez l'élément HTML où vous voulez afficher les détails
    const detailElement = document.querySelector('.detail');
    // Mettez à jour son contenu avec les détails
    detailElement.innerHTML = affichage;
  });
};



</script>

<style scoped>
.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
}

</style>