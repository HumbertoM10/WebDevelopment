<style>
  button {
      background-color: #FFFFFF;
      -webkit-transition-duration: 0.4s; /* Safari */
      transition-duration: 0.4s;
      border-radius: 12px;
  }

  button:hover {
      background-color: #4CAF50; /* Green */
      color: white;
  }

  input[type=submit]{
    background-color: #33bbff;
    border: none;
    color: white;
    padding: 16px 32px;
    text-decoration: none;
    margin: 4px 2px;
    cursor: pointer;
  }
  form{
    text-align: center;
  }
  div.ex1 {
    width: 500px;
    margin: auto;
    background-color: #99c5d0;
    font-family: Georgia;
  }
  body {
    background-color: lightblue;
  }
  h1{
    text-align: center;
  }
</style>

<template>
  <div class="ex1">
    <h1>My ToDo App</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTaskName" type="text">
      <input type="submit" value="Agregar Tarea">
    </form>

    <tasks-list
      :title="'Pendientes'"
      :task-list="tasksPending"
      @completar="toggleTasks"/>
    <hr>

    <tasks-list
      :title="'Completados'"
      :task-list="tasksComplete"
      @completar="toggleTasks"/>
  </div>
</template>

<script>
import VueLocalStorage from 'vue-localstorage'
import TasksList from './components/TasksList'
const STORAGE_KEY = 'storage';

//Vue.use(VueLocalStorage)  
export default {
  components: {
    TasksList
  },
  data () {
    return {
      newTaskName: '',
      tasks: []
    }
  },
  created () {
      this.tasks = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
  },
  mounted() {
    const tasks = JSON.parse(this.$localStorage.get('tasks'))
    if (tasks) {
      this.tasks = tasks
    }
  },
  watch: {
    input: function () {
     if (isLocalStorage() /* function to detect if localstorage is supported*/) {
       localStorage.setItem('tasks', this.tasks)
     }
   }
  },
  methods: {
    toggleTasks (task) {
      task.done = !task.done
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
    },
    addTask () {
      if (!this.newTaskName) return
      this.tasks.push({ name: this.newTaskName, done: false })
      this.newTaskName = ''
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
    }
  },
  computed: {
    tasksPending () {
      return this.tasks.filter(task => !task.done)
    },
    tasksComplete () {
      return this.tasks.filter(task => task.done)
    }
  }
}
</script>