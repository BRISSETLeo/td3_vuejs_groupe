<template>
  <div>
    <h2>{{ mode === 'edit' ? 'Modifier' : 'Créer' }} un questionnaire</h2>
    <form @submit.prevent="submitForm">
      <label>Titre:</label>
      <input type="text" v-model="name" required>
      <button type="submit">{{ mode === 'edit' ? 'Modifier' : 'Créer' }}</button>
      <button type="button" @click="$emit('annuler')">Annuler</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ['mode', 'questionnaire', 'annuler'],
  data() {
    return {
      name: this.mode === 'edit' ? this.questionnaire.title : ''
    }
  },
  methods: {
    submitForm() {
      const formData = {
        name: this.name
      };
      this.$emit(this.mode === 'edit' ? 'editQuestionnaire' : 'createQuestionnaire', formData);
    },
  }
}
</script>
