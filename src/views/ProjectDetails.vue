<template>
  <div  class="project" :class="{ complete: project.complete }" >
    <div class="actions">
        <h3 @click="toggleDetails">{{ project.title }}</h3>
        <div v-if="showIcons" class="icons">
            <router-link :to=" {name: 'EditProject', params: { id: project.id }}">
                <span class="material-symbols-outlined">edit</span>
            </router-link>
            <span @click="completeProject" class="material-symbols-outlined tick">check</span>
            <span @click="showDeleteConfirmation" class="material-symbols-outlined">delete</span>
        </div>

        <div v-if="showConfirmation" id="delete-confirmation" class="delete-confirmation">
                <button class="btn1" @click="deleteProject">confirm</button>
                <button class="btn2" @click="hideDeleteConfirmation">cancel</button>
        </div>
        
    </div>


    <div v-if="showDetails">
        <p> {{ project.details }}</p>
    </div>

  


  
</div>
</template>z

<script>
export default {
    props:['project'],
    data(){
        return{
            showDetails: false,
            showConfirmation: false,
            showIcons:true,
            uri:'http://localhost:3000/projects/' + this.project.id
        }
    },
    methods:{
        toggleDetails(){
            this.showDetails=!this.showDetails
        },
        deleteProject(){
            fetch(this.uri,{method:'DELETE'})
            .then(()=>this.$emit('delete',this.project.id))
        },
        completeProject(){
            fetch(this.uri,{
                method:'PATCH',
                headers:{'Content-Type':'application/json'},
                body: JSON.stringify({complete: !this.project.complete})})
            .then(()=>this.$emit('complete',this.project.id))
        },
        showDeleteConfirmation(){
            this.showConfirmation=true;
            this.showIcons=false;
        },
        hideDeleteConfirmation(){
            this.showConfirmation=false;
            this.showIcons=true;
        }
    }
}
</script>

<style scoped>

  .delete-confirmation{
    float: inline-end;
  }
  .btn1{
    margin-right: 5px;
  }
  .btn2{
    margin-left: 5px;

  }

  .project {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 6px solid #e90074;
  }
  h3 {
    cursor: pointer;
  }
  .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .material-symbols-outlined {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
  }
  .material-symbols-outlined:hover {
    color: #777;
  }
  /* completed projects */
  .project.complete {
    border-left: 6px solid #00ce89;
  }
  .project.complete .tick {
    color: #00ce89;
  }
  button{
    margin: 20px auto 0;
    background: #e9366f;
    color: white;
    padding: 5px;
    border: 0;
    border-radius: 6px;
    font-size: 12px;
  }
</style>