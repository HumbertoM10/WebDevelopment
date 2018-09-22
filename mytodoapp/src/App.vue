<template>
  <div>
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
import TasksList from './components/TasksList'

export default {
  components: {
    TasksList
  },
  data () {
    return {
      newTaskName: '',
      tasks: [
        {
          name: 'Tarea 1',
          done: false
        },
        {
          name: 'Tarea 2',
          done: false
        },
        {
          name: 'Tarea 2',
          done: false
        }
      ]
    }
  },
  methods: {
    toggleTasks (task) {
      task.done = !task.done
    },
    addTask () {
      if (!this.newTaskName) return
      this.tasks.push({ name: this.newTaskName, done: false })
      this.newTaskName = ''
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