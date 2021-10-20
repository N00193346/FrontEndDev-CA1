<template>
  <div>
      <router-link :to="{name: 'home'}">Home</router-link>  |
      <router-link :to="{name: 'all_countries'}">All</router-link>
  </div>
</template>

<script>
    export default {
        name: 'NavBar'
    }
</script>

<style>

</style>
