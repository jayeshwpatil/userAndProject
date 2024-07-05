<template>
  <base-container v-if="user">
    <h2>{{ user.fullName }}: Projects</h2>
    <base-search v-if="hasProjects" @search="updateSearch" :search-term="enteredSearchTerm"></base-search>
    <ul v-if="hasProjects">
      <project-item v-for="prj in availableProjects" :key="prj.id" :title="prj.title"></project-item>
    </ul>
    <h3 v-else>No projects found.</h3>
  </base-container>
  <base-container v-else>
    <h3>No user selected.</h3>
  </base-container>
</template>

<script>
import ProjectItem from './ProjectItem.vue';
import {computed, ref, watch, toRefs} from 'vue';

export default {
  components: {
    ProjectItem,
  },
  props: ['user'],
  setup(props){
    let enteredSearchTerm = ref('')      //what ever user enter in project search box
    let activeSearchTerm = ref('')        //user project search item

    function updateSearch(val){
      enteredSearchTerm.value = val;
    }

    let availableProjects = computed(()=>{
      if (activeSearchTerm.value) {
        console.log("active user value is ",  activeSearchTerm.value)
        return props.user.projects.filter((prj) =>
          prj.title.toLowerCase().includes(activeSearchTerm.value.toLowerCase())
        );
      }
      return props.user.projects;
    })
    console.log("active project is " , availableProjects)

    let hasProjects = computed(()=>{
      return props.user.projects && availableProjects.value.length > 0;
    })

    watch(enteredSearchTerm, function(val){
      // setTimeout(() => {
        if (val === enteredSearchTerm.value) {
          activeSearchTerm.value = val;
        }
      // }, 2000);
    })

    let {user} = toRefs(props)      //convert props to refs using object destructuring

    watch(user, ()=>{
      enteredSearchTerm.value = '';
    })

    return {enteredSearchTerm, activeSearchTerm, updateSearch, availableProjects, hasProjects}
  }

};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>