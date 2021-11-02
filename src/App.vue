<template>
  <div class="app">
        <NavBar/>
      
     <transition name="fade" mode="out-in"> 
    <router-view :key="$route.fullPath" />
     </transition>
  </div>
</template>

<script>
import NavBar from '@/components/NavBar.vue'

export default {
  name: 'App',
  components: {
    NavBar,
  }
}
</script>

<style>



.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-fom,
.fade-leave-to {
  opacity: 0;
}

</style>
