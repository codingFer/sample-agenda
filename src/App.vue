<template>
  <section style="display: flex; justify-content: center">
    <img alt="Vue logo" src="./assets/logo.png" style="text-align: center">
  </section>
  <AgendaView/>
</template>

<script>
import AgendaView from './views/Agenda.view.vue';

export default {
  name: 'App',
  components: {
    AgendaView
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
