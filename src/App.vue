<template>
  <div id="app" class="container mt-4">
    <Navbar />
    <Logo />
    <Search />
    <ul class="nav justify-content-center">
      <li class="nav-item">
          <span class="badge bg-primary">Os resultados podem ser imprecisos</span>
      </li>
    </ul>
  </div>
</template>

<script>
import Logo from './components/Logo.vue'
import Navbar from './components/Navbar.vue'
import Search from './components/Search.vue'
export default {
  name: 'App',
  components: { 
    Search, 
    Logo,
    Navbar 
  }
}
</script>

<style>
</style>
