<template>
    <div class="section-container align-self-start d-flex flex-column p-2 m-2">
        <div class="m-2">
            <input v-bind:value="section.name" v-on:input="$emit('input', $event.target.value)" type="text" class="form-control" placeholder="Section Name" aria-label="Section Name">
        </div>
        <task-card 
            v-for="task in section.tasks"
            v-bind:task='task'
            v-bind:key='task.id'
            v-bind:section_id='section.id'
            v-bind:sections="sections"
            v-on:task-name-input="taskNameInput(task, $event)"
            v-on:task-content-input="taskContentInput(task, $event)"
            v-on:change-section="changeSection"
            v-on:change-form="changeForm"
            v-on:change-state="changeState"
            v-on:change-importance="changeImportance"
            v-on:delete-task="deleteTask"
        ></task-card>
        <div>
            <button v-on:click="$emit('add-task', section.id)" class="btn btn-light">+ Add Task</button>
        </div>
    </div>
</template>

<script>
import TaskCard from './TaskCard.vue'

export default {
	name: 'TaskSection',
	components: {
		'task-card': TaskCard
	},
	props: {
		section: Object,
		sections: Array
	},
    methods: {
        taskNameInput(task, input){
            task.name = input;
            console.log("task name: "+task.name);
        },
        taskContentInput(task, input){
            task.content = input;
            console.log("task content: "+ task.content);
        },
        changeSection(newSectionId, oldSectionId, task){
            this.$emit('change-section', newSectionId, oldSectionId, task);
        },
        changeForm: function(task){
            task.wMode = !task.wMode;
        },
        changeState: function(task){
            task.isDone = !task.isDone;
        },
        changeImportance: function(task){
            task.isImportant = !task.isImportant;
        },
        deleteTask: function(sectionId, taskId){
            this.$emit('delete-task', sectionId, taskId);
        }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.section-container{
    min-width: 300px;
    border: 1px solid lightgrey;
    border-radius: 3px;
    box-shadow: 2px 2px 2px grey; 
}
button{
    box-shadow: 1px 1px 2px black;
}
</style>
