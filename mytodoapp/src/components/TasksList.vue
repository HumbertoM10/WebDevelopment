<template>
  <div>
    <h3>{{ title }}</h3>
      <ul>
        <li v-for="(task, index) in taskList"
            :key="index">
          {{ task.name }} - {{ task.done ? 'Completado' : 'Pendiente' }}
          <button v-if="!task.done" type="button" @click="toggleTasks(task)">Completar</button>
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    taskList: {
      type: Array,
      required: true
    }
  },
  methods: {
    toggleTasks (task) {
      this.$emit('completar', task)
    }
  }
}
</script>