<template>
    <p @click="afficherDetail">{{ questionnaire.name }}</p>
    <button @click="remove">Supprimer</button>
    <button @click="modifier">Modifier le nom du questionnaire</button>
</template>

<script>
export default {
  props: {
    questionnaire: Object
  },
  methods: {
    remove() {
      this.$emit('remove', { id: this.questionnaire.id });
    },
    modifier() {
      this.$emit('modifier', { id: this.questionnaire.id });
    },
    afficherDetail(){
      this.$emit('afficherDetail', { id: this.questionnaire.id });
    }
  },
  emits: ['remove', 'modifier', 'afficherDetail']
}
</script>

<style scoped>
li {
  margin-bottom: 10px;
}
button {
  font-size: 0.8em;
  color: red;
}
</style>