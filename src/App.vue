<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
   <div class="container">
    <header>
      <div class="wrapper">
        <nav>
          <RouterLink to="/">Home</RouterLink> |
          <RouterLink to="/about">About</RouterLink> |
          
          <RouterLink to="/week2">第二週作業</RouterLink> |
          
        </nav>
      </div>
    </header>
    
    <RouterView />
  </div>
     

  <RouterView />
</template>

<style scoped>

</style>
