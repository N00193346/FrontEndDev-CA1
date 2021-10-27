<template>
  <div class="container">
 
        <CountryCard 
      v-for="country in countries"
      :key = "country.ccn3"
      :country="country"/>
  </div>
</template>

<script>
import axios from 'axios'
import CountryCard from '@/components/CountryCard'
export default {
  name: 'Country',
       components: {
            CountryCard,
        },
        data () {
            return {
                countries: []
            }
        },
        mounted(){
          axios.get(`https://restcountries.com/v3.1/name/${this.$route.params.country}`)
              .then(response => {
                console.log(response.data)
                this.countries = response.data
              })
              .catch(error => console.log(error))
        }
}
</script>

<style>
.container{
  display: flex;
  align-content: flex-start;
  flex-wrap: wrap ;
}
</style>
