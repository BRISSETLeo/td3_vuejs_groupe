<script>
export default {
  props: {
    task: Object
  },
  methods: {
    remove() {
      this.$emit('remove',{id:this.task.id}) ;
    },
    modifier(){
      this.$emit('modifier',{id:this.task.id}) ;
    }
  },
  emits: ['remove', 'modifier']
}
</script>

<template>
  <li>
    {{ task.title }}
    <button @click="remove">Supprimer</button>
    <button @click="modifier">Modifier</button>
  </li>
</template>
