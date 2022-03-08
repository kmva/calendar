<template>
  <Calendar :events="events"/>
</template>

<script>
import Calendar from './components/Calendar.vue'
import events from './data/events'

export default {
  name: 'App',
  components: {
    Calendar
  },
  setup() {
    return events
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
