<template>
  <div>
    <TaskManager />
  </div>
</template>

<script>
import TaskManager from './components/TaskManager.vue';

export default {
  name: 'App',
  components: {
    TaskManager
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top: 60px;
}
</style>
