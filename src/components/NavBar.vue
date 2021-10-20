<template>
  <div>
      <router-link :to="{name: 'home'}">Home</router-link>  |
      <router-link :to="{name: 'all_countries'}">All</router-link> |
      <router-link :to="{name: 'search_countries'}">Search</router-link>
  </div>
</template>

<script>
    export default {
        name: 'NavBar'
    }
</script>

<style>

</style>
