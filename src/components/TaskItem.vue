<template>
  <ul class="list-group list-group-flush">
    <li class="list-group-item" v-for="task in tasks" v-bind:id="task.id" v-bind:class="{'warning': !task.saved}">
      <!-- TASK DETAILS -->
      <h6>{{ task.title }}</h6>
      <p>{{ task.description }}</p>
      <a v-on:click.prevent="filterByCategory" href="#">{{ task.category }}</a>
      <p>{{ task.date }}</p>
      <br>
      <!-- UNSAVED CHANGES ALERT -->
      <div v-if="!task.saved" class="alert alert-danger" role="alert">
        If you have any unsaved changes, they will be lost!
      </div>
      <!-- EDIT/REMOVE BUTTONS -->
      <button v-on:click="editTask(task.id)" class="btn btn-warning">Edit</button>
      <button v-on:click="removeTask(task.id)" class="btn btn-danger">Remove</button>
    </li>
  </ul>
</template>

<script>
export default {
  data: function() {
    return {
    }
  },
  props: ['tasks', 'taskToEdit', 'taskInitialState'],
  methods: {
    removeTask(id) {
      let taskToRemoveIndex = this.tasks.findIndex(x => x.id === id);
      this.tasks.splice(taskToRemoveIndex, 1);
      // TODO: Вынести в отдельную функцию, как часто используемую
      localStorage.setItem('tasks', JSON.stringify({'tasks': [...this.tasks]}));
    },
    // TODO: ПЕРЕДЕЛАТЬ!
    editTask(id) {
      let taskToEditIndex = this.tasks.findIndex(x => x.id === id);
      let taskToEditData = this.tasks[taskToEditIndex];
      taskToEditData.saved = false;
      this.$emit('editButtonWasClicked', taskToEditData);
      $('#editTaskModal').modal('show');
    },
    // TODO: такая же функция в компоненте с категориями
    filterByCategory(evt) {
      let current_category = evt.target.textContent;
      let filtered_tasks = JSON.parse(localStorage.getItem('tasks')).tasks.filter(x => x.category === current_category);
      this.$emit('tasksWasFiltered', filtered_tasks);
    },
  },
}
</script>

<style>
  .warning {
    background-color: #F2EAEA !important;
  }
</style>
