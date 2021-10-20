<template>
  <div>
      <p><b>Flag:</b>{{ country.flag }} </p>
      <p><b>Name:</b>{{ country.name.common }}</p>
       <p><b>Capital:</b>{{ country.capital[0]}}</p>
       <hr>
  </div>
</template>

<script>
    export default {
        name: 'CountryViewer',
        props: {
            country: Object
        }
    }
</script>

<style>

</style>
