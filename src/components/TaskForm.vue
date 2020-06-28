<template>
  <div class="row">
    <div class="col-md-12">
      <form>
        <!-- TASK TITLE -->
        <div class="form-group">
          <input v-if="!taskToEdit" v-model="newTask.title" type="text" class="form-control" placeholder="Task title" required>
          <input v-else v-model="taskToEdit.title" type="text" class="form-control" placeholder="Task title" required>
        </div>
        <!-- TASK DESCRIPTION -->
        <div class="form-group">
          <input v-if="!taskToEdit" v-model="newTask.description" type="text" class="form-control" placeholder="Description">
          <input v-else v-model="taskToEdit.description" type="text" class="form-control" placeholder="Description">
        </div>
        <!-- TASK CATEGORY -->
        <div class="form-group">
          <select v-if="!taskToEdit" v-model="newTask.category" class="form-control">
            <option v-for="category in categories">{{ category }}</option>
          </select>
          <!-- CATEGORIES LIST -->
          <select v-else v-model="taskToEdit.category" class="form-control">
            <option selected>{{ taskToEdit.category }}</option>
            <option v-for="category in categories">{{ category }}</option>
          </select>
        </div>
        <!-- TASK DATE -->
        <div class="form-group">
          <input v-if="!taskToEdit" v-model="newTask.date" type="date" class="form-control">
          <input v-else v-model="taskToEdit.date" type="date" class="form-control">
        </div>
        <!-- SAVE/CANCEL BUTTONS -->
        <button v-if="taskToEdit" v-on:click.prevent="saveEditedTask(taskToEdit.id)" class="btn btn-success">Save Task</button>
        <button v-else v-on:click.prevent="saveNewTask" class="btn btn-success">Add Task</button>
        <button v-if="taskToEdit" v-on:click.prevent="cancelEditedTaskChanges()" class="btn btn-danger">Cancel Last Changes</button>
      </form>
    </div>
  </div>
</template>

<script>
import create_UUID from '../tools/uuid.js';

export default {
  data: function() {
    return({
      newTask: {},
    })
  },
  props: ['categories', 'tasks', 'taskToEdit', 'taskInitialState'],
  methods: {
    saveNewTask() {
      if (this.newTask.title) {
        this.newTask.id = create_UUID();
        this.newTask.saved = true;
        this.tasks.push({...this.newTask});
        localStorage.setItem('tasks', JSON.stringify({'tasks': [...this.tasks]}));
        this.newTask = {};
      } else {
        // TODO: всплывающую подсказку или модальное окно
        alert('Введите заголовок')
      }
    },
    saveEditedTask(id) {
      let taskToEditIndex = this.tasks.findIndex(x => x.id === id);
      this.tasks.splice(taskToEditIndex, 1, this.taskToEdit);
      this.taskToEdit.saved = true;
      localStorage.setItem('tasks', JSON.stringify({'tasks': [...this.tasks]}));
      $('#editTaskModal').modal('hide');
    },
    cancelEditedTaskChanges() {
      console.log(this.taskInitialState);
      this.$emit('taskEditChangesWasCanceled', this.taskInitialState);
    }
  },
}
</script>

<style>

</style>
