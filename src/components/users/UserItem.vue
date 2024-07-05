<template>
  <li>
    <h3>{{ userName }}</h3>
    <button @click="viewProjects">View Projects</button>
  </li>
</template>

<script>

export default {
  props: ['id', 'userName'],
  emits: ['list-projects'],

  setup(props, { emit }){
    function viewProjects(){
      emit('list-projects', props.id);
    }

    return {viewProjects}
  }
};
</script>

<style scoped>
li {
  margin: 20px 0; background: #f4f4f4;
  padding: 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.26);
  border: 1px solid #ccc;
}

li h3 {
  margin: 0;
}
</style>