<template>
    <div class="task-container p-2 my-2">
        <div class="my-2">
            <input v-bind:value="task.name" v-on:input="$emit('task-name-input', $event.target.value)" type="text" class="form-control" placeholder="Task Name" aria-label="Task Name">
        </div>
        <div class="form-floating mb-2">
            <select v-bind:value="section_id" v-on:change="$emit('change-section', $event.target.value, section_id, task)" class="form-select" id="floatingSelect">
                <option v-for="section in sections" v-bind:key="section.id" v-bind:value="section.id">
                    {{ section.name }}
                </option>
            </select>
            <label for="floatingSelect">Section Name</label>
            </div>
        <div>
            <textarea v-if="task.wMode" v-bind:value="task.content" v-on:input="$emit('task-content-input', $event.target.value)"></textarea>
            <p v-else>{{ task.content }}</p>
        </div>
        <div class="d-flex justify-content-end">
            <button v-on:click="$emit('change-form', task)" class="icon">
                <i class="fas fa-sticky-note fa-lg"></i>
            </button>
            <button v-on:click="$emit('change-state', task)" class="icon" v-bind:style="{ color: checkColor(task) }">
                <i class="fas fa-check-square fa-lg"></i>
            </button>
            <button v-on:click="$emit('change-importance', task)" class="icon" v-bind:style="{ color: starColor(task) }">
                <i class="fas fa-star fa-lg"></i>
            </button>
            <button v-on:click="$emit('delete-task', section_id, task.id)" class="icon">
                <i class="fas fa-trash-alt fa-lg"></i>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TaskCard',
    props: {
        task: Object,
        section_id: Number,
        sections: Array
    },
    methods: {
        starColor: function(task){
            return task.isImportant ? "yellow" : "grey";
        },
        checkColor: function(task){
            return task.isDone ? "red" : "grey";
        }
    }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.task-container{
    overflow-wrap: anywhere;
    border: 1px solid lightgrey;
    border-radius: 3px;
    box-shadow: 2px 2px 2px grey; 
}
.icon{
    background-color: white;
    box-shadow: none;
    text-align: center;
    border: none;
    height: 35px;
    width: 35px;
    color: grey;
    border-radius: 50%;
}
</style>