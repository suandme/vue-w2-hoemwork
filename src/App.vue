<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
   <div class="container">
    <header>
      <div class="wrapper">
        <nav>
          <RouterLink to="/">Home</RouterLink> |
          <RouterLink to="/WorkViewOne">第一週作業</RouterLink> |
          <RouterLink to="/week2">第二週作業</RouterLink> |
          
        </nav>
      </div>
    </header>
    
    <RouterView />
  </div>
     
</template>
