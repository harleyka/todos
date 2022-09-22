<template>
  <main>
    <el-row class="mb-4">
      <h1>TODO App</h1>
    </el-row>
    

    <el-row class="mb-4">
      <el-button v-if="!showForm" @click="showForm = !showForm" type="info">Nový úkol</el-button>

      <transition>
        <todo-form v-if="showForm" @add="addToData" @cancel="showForm = false"></todo-form>
      </transition>
    </el-row>

    <div v-for="item in data" :key="item.id">
      <todo-item v-show="showAllTodos || (!showAllTodos && !item.finished)" v-bind="item" @finish="finishTodo" @delete="deleteTodo"></todo-item>
    </div>  
    

    <el-checkbox @change="showAllTodos = !showAllTodos">Pouze nedokončené</el-checkbox>
  </main>
</template>


<script>
import TodoItem from './components/TodoItem.vue'
import TodoForm from './components/TodoForm.vue'
import data from './data.json'

export default {
  name: 'app',
  components: { TodoItem, TodoForm },
  data() {
    return {
      data: null,
      showForm: false,
      showAllTodos: true,
    }
  },
  computed: {
    unfinishedTodos() {
      return this.data.filter((item) => !item.finished);
    },
  },
  methods: {
    getTodoById(id) {
      return this.data.findIndex((item) => item.id == id);
    },
    sort() {
      return this.data.sort((a, b) => {
        let priorities = {
          'low': 2,
          'normal': 1,
          'high': 0
        };
        return (a.finished * 10 + priorities[a.priority]) - (b.finished * 10 + priorities[b.priority]);
      });
    },
    finishTodo(id) {
      let index = this.getTodoById(id);
      this.data[index].finished = true;
      this.sort();
    },
    deleteTodo(id) {
      let index = this.getTodoById(id);
      this.data.splice(index, 1);
      this.sort();
    },
    addToData(todo) {
      todo.id = this.data.length;
      this.data.push(todo);
      this.showForm = false;
      this.sort();
    },

  },
  mounted() {
    this.data = data;
    this.sort();
  }
}
</script>


<style scoped>
.v-enter-active {
  transition: opacity 0.5s ease;
}

.v-enter-from {
  opacity: 0;
}
</style>
