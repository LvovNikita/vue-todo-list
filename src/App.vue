<template>
  <div class="container">
    <navigation></navigation>
    <!-- CATEGORIES CMP -->
    <categories
      v-bind:categories="categories"
      v-bind:tasks="tasks"
      v-on:tasksWasFiltered="tasks = $event"
      v-on:filterWasReseted="tasks = $event"
    ></categories>
    <!-- TASK ITEM CMP -->
    <task-item
      v-bind:tasks="tasks"
      v-bind:taskInitialState="taskInitialState"
      v-bind:taskToEdit="taskToEdit"
      v-on:editButtonWasClicked="
        taskToEdit = $event;
        taskInitialState = {...$event}"
      v-on:tasksWasFiltered="tasks = $event"
    ></task-item>
    <!-- TASK FORM CMP -->
    <task-form
      v-bind:categories="categories"
      v-bind:tasks="tasks"
    ></task-form> <!-- CMP -->
    <!-- MODAL -->
    <!-- TODO: Вынести в отдельный компонент  -->
    <div id="editTaskModal" class="modal">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <task-form
              v-bind:tasks="tasks"
              v-bind:taskToEdit="taskToEdit"
              v-bind:categories="categories"
              v-bind:taskInitialState="taskInitialState"
              v-on:taskEditChangesWasCanceled="taskToEdit = {...$event}"
              ></task-form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

  // CONSTRUCTORS
  class Task {
    constructor(id, title, description, category, date) {
      this.id = id;
      this.title = title;
      this.description = description;
      this.category = category;
      this.date = date;
      this.saved = true
    }
  }

  // DATA
  let tasksFromLocalStorage = JSON.parse(localStorage.getItem('tasks'))
    ? JSON.parse(localStorage.getItem('tasks'))['tasks']
    : [new Task('1', 'Test task', 'Your first task', 'Inbox', '2020-02-02')]

  let categoriesFromLocalStorage = JSON.parse(localStorage.getItem('categories'))
    ? JSON.parse(localStorage.getItem('categories'))['categories']
    : ['Inbox'];

  // COMPONENTS
  import Navigation from './components/Navigation.vue';
  import Categories from './components/Categories.vue';
  import TaskItem from './components/TaskItem.vue';
  import TaskForm from './components/TaskForm.vue';

  // VUE
  export default {
    data: function() {
      return {
        tasks: tasksFromLocalStorage,
        categories: categoriesFromLocalStorage,
        taskToEdit: {},
        taskInitialState: {}
      }
    },
    components: {
      Navigation,
      Categories,
      TaskItem,
      TaskForm
    }
  }
</script>

<style>
  body {
    padding: 2em;
    background-color: #eeeefe
  }
  h1 {
    color: #AAAAAA
  }
  p {
    margin-bottom: 0;
  }
  li {
    background-color: #fafafa !important;
    margin-bottom: 0.5em;
    box-shadow: 0 0 7px rgba(0,0,0,0.1);
  }
  .btn-group-sm button {
    border-radius: 0px !important;
  }
</style>
