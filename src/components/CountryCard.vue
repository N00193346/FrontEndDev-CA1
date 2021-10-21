<template>
<div>
  
  <b-row>
    <b-col>
    <b-card
    :title= country.name.common 
     :img-src= country.coatOfArms.png
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem;"
    class="mb-2"
  >
    <b-card-text>
     <b>Capital City: </b>{{ country.capital[0]}}
     <hr>
      <b>Languages: </b>{{ country.languages}}
        <hr>
      <b>Currency: </b> {{ country.currencies}}
        <hr>
       <b>Population: </b>  {{ country.population}}
    </b-card-text>

    <b-button href="#" variant="primary">View Country</b-button>
  </b-card>
    </b-col>

  </b-row>
 
</div>
</template>

<script>
    export default {
        name: 'CountryCard',
        props: {
            country: Object
        }
    }
</script>

<style>

</style>
