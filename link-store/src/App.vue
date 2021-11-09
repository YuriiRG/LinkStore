<template>
  <nav-bar/>
  <main-layout/>
</template>

<script>
import MainLayout from './components/MainLayout.vue'
import NavBar from './components/NavBar.vue'
export default {
  name: 'App',
  components: {
    MainLayout,
    NavBar
  }
}
</script>