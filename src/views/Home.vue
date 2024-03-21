<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <ProjectDetails :project="project" @delete="handleDelete" @complete="handleComplete"/>
        <p></p>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import ProjectDetails from '@/views/ProjectDetails.vue'

export default {
  name: 'Home',
  components: { ProjectDetails },
  data(){
    return{
      projects:[],
      uri:'http://localhost:3000/projects'
    };
  },
  mounted(){
    fetch(this.uri)
    .then(res=>res.json())
    .then(data => this.projects=data)
  },
  methods:{
    handleDelete(id){
      this.projects=this.projects.filter(project =>{
        return project.id!==id
      })
    },
    handleComplete(id){
      let p = this.projects.find(project => {
        return project.id === id
      })
      p.complete = !p.complete 
    }

  }

  
}
</script>

