<template>
  <div>
    <h2>{{ mode === 'edit' ? 'Modifier' : 'Créer' }} une question pour le questionnaire n°{{ questionnaireId }}</h2>
    <form @submit.prevent="submitForm">
      <label>Contenu:</label>
      <input type="text" v-model="title" required>
      <label>Question type:</label>
      <input type="text" v-model="questionType" required>
      <label>Réponse</label>
      <input type="text" v-model="answer" required>
      <button type="submit">{{ mode === 'edit' ? 'Modifier' : 'Créer' }}</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ['mode', 'question', 'questionnaireId'],
  data() {
    return {
      title: this.mode === 'edit' ? this.question.title : '',
      questionType: this.mode === 'edit' ? this.question.questionType : '',
      answer: this.mode === 'edit' ? this.question.answer : ''
    }
  },
  methods: {
    submitForm() {
      const formData = {
        title: this.title,
        questionType: this.questionType,
        answer: this.answer
      };
      const questionnaireId = this.questionnaireId;
      this.$emit(this.mode === 'edit' ? 'editQuestion' : 'createQuestion', questionnaireId, formData);
    }
  }
}
</script>
