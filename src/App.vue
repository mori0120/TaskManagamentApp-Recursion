<template>
  <div class="d-flex m-3" id="app">
    <task-section
      v-for="section in sections"
      v-bind:key="section.id"
      v-bind:section="section" 
      v-bind:sections="sections"
      v-on:change-section="changeSection"
      v-on:add-task="addTask"
      v-on:delete-task="deleteTask"
      v-model="section.name"
    ></task-section>
    <div>
      <button v-on:click="addSection" class="btn btn-light" style="min-width: 130px;">+ Add Section</button>
    </div>
  </div>
</template>

<script>
import TaskSection from './components/TaskSection.vue'

export default {
  name: 'App',
  components: {
    'task-section': TaskSection
  },
  data: function(){
    return {
      sections:[
        {   //section obj
          id: 1, 
          name: "" ,
          taskIdCounter: 1,
          tasks: [ 
            //task obj
            { id: 1, name: "", content: "", wMode: false, isDone: false, isImportant: false },
          ]
        }
      ]     
    }
  },
  methods:{
    addSection: function(){
      let newId = this.sections.length+1;
      let newSection = { 
        id: newId,
        name: "" ,
        taskIdCounter: 1,
        tasks: [
          { id: 1, name: "", content: "", wMode: false, isDone: false, isImportant: false },
        ]
      };
      this.sections.push(newSection);
    },
    changeSection(newSectionId, oldSectionId, task){
      let currentSection = this.sections[oldSectionId - 1];
      let nextSection = this.sections[newSectionId - 1];
      currentSection.tasks = currentSection.tasks.filter(t => t!==task);
      task.id = ++nextSection.taskIdCounter;
      nextSection.tasks.push(task);
    },
    addTask: function(sectionId){
      let section = this.sections[sectionId-1];
      let newTask = { id: ++section.taskIdCounter, name: "", content: "", wMode: false, isDone: false, isImportant: false };
      section.tasks.push(newTask);
    },
    deleteTask: function(sectionId, taskId){
      let section = this.sections[sectionId-1];
      section.tasks = section.tasks.filter(task => task.id !== taskId);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button{
  box-shadow: 1px 1px 2px black;
}
</style>
