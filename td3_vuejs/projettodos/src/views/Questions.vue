<template>
  <div>
    <ul>
      <li v-if="question" :key="question.id">
        {{ question.title }}
        <button @click="editQuestion()">Modifier</button>
        <button @click="deleteQuestion()">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    question: Object,
  },
  methods: {
    editQuestion() {
      this.$emit('editQuestion', this.question.id);
    },
    deleteQuestion() {
      this.$emit('deleteQuestion', this.question.id);
    }
  },
  emits: ['editQuestion', 'deleteQuestion']
}
</script>
