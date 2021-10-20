<template>
  <div >

      This is the home page

      <SearchCountry /> 

  </div>
</template>

<script>
import SearchCountry from "@/components/SearchCountry.vue";
    export default {
        name: 'Home',
        components: {
            SearchCountry,
        },
    }
</script>

<style>

</style>
