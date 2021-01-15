<template>
  <div id="app">
    <header>
      <div class="title">{{title}}</div>
    </header>

    <section class="main-content w-50 mx-auto">
      <main-navigation @update-display="updateDisplay"></main-navigation>
      <windows-list v-if="display ==='windows'"></windows-list>
      <room-list v-if="display ==='rooms'"></room-list>
    </section>

  </div>
</template>

<script>
import MainNavigation from './components/MainNavigation.vue';
import WindowsList from './components/WindowsList.vue';
import RoomList from './components/RoomList.vue';

export default {
  name: 'App',
  components: {
    MainNavigation,
    WindowsList,
    RoomList
  },
  data: function() {
    return {
      title: 'Smart Building Management',
      display: "windows"
    }
  },
  methods: {
    updateDisplay(choice) {
      this.display=choice;
    }
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
