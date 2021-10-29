<template>

 
    <router-link style="text-decoration: none; color: inherit;" :to="{name: 'country', params: {country: country.name.common}}">
    <b-card
    :title= country.name.common 
     :img-src= country.flags.png
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

  </b-card>
  </router-link >

 

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
.card{
  margin: 20px;

}
</style>
