<template>
  <div>
    <div class="btn-group-sm">
      <!-- CATEGORIES -->
      <button
        v-for="category in categories"
        v-on:click.exact="filterByCategory"
        v-on:click.ctrl="removeCategory"
        class="btn btn-secondary category_btn"
      >{{category}}</button>
      <!-- FILTER RESET -->
      <button
        v-on:click="resetFilter"
        class="btn btn-secondary btn-danger"
      >Reset Filter</button>
    </div>
    <!-- ADD CATEGORY FORM -->
    <form v-on:submit.prevent="addCategory">
      <div class="form-group">
        <input v-model="newCategory" type="text" class="form-control" required>
      </div>
      <button class="btn btn-success">Add Category</button>
    </form>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      newCategory: ''
    }
  },
  props: ['categories', 'tasks'],
  methods: {
    addCategory() {
      if (!this.categories.includes(this.newCategory)) {
        this.categories.push(this.newCategory);
        localStorage.setItem('categories', JSON.stringify({'categories': [...this.categories]}));
      };
      this.newCategory = '';
    },
    removeCategory(evt) {
      let current_category = evt.target.textContent;
      // TODO: вынести в функцию updateLocalStorageItem
      let categoryToRemoveIndex = this.categories.findIndex(x => x === current_category);
      this.categories.splice(categoryToRemoveIndex, 1);
      localStorage.setItem('categories', JSON.stringify({'categories': [...this.categories]}));
    },
    // TODO: объединить с resetFilter в одну функцию
    filterByCategory(evt) {
      let current_category = evt.target.textContent;
      let category_buttons = document.querySelectorAll('.category_btn');
      // TODO: повторяется в функции resetFilter
      for (let btn of category_buttons) {
        btn.classList.remove('active');
      };
      evt.target.classList.add('active');
      let filtered_tasks = JSON.parse(localStorage.getItem('tasks')).tasks.filter(x => x.category === current_category);
      this.$emit('tasksWasFiltered', filtered_tasks);
    },
    resetFilter() {
      let category_buttons = document.querySelectorAll('.category_btn');
      for (let btn of category_buttons) {
        btn.classList.remove('active');
      };
      let all_tasks = JSON.parse(localStorage.getItem('tasks')).tasks;
      this.$emit('filterWasReseted', all_tasks);
    },
  }
}
</script>

<style scoped>
  .btn-group-sm, form {
    margin-bottom: 1em;
  }
  .btn-secondary {
    margin-bottom: 5px;
  }
</style>
