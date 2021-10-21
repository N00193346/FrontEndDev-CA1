<template>
  <!-- <div >
    
      <CountryCard 
      v-for="country in countries"
      :key = "country.ccn3"
      :country="country"/>
     
  </div> -->

    <div>
        <CountryCard
        :country=countries 
        />
    </div>
</template>
     

<script>

import axios from 'axios'
import CountryCard from '@/components/CountryCard'


    export default {
        name: 'AllCountries',
        components: {
            CountryCard,
        },
        data () {
            
            return {
                countriesLength: null,
                randomCountryNum: null,
                countries: [],
               
            }
        },
        mounted () {
            axios.get('https://restcountries.com/v3.1/all')
                 .then(response => {
                     console.log(response)
                     this.countries = response.data
                    // this.randomCountry();
                     this.countriesLength = this.countries.length
                     console.log("The length of the array is " + this.countriesLength)
                     this.randomCountryNum = (Math.round(Math.random()*this.countriesLength))
                     console.log("The number of the random country is  " + this.randomCountryNum)
                     this.countries = response.data[this.randomCountryNum]
                     })
                 .catch(error => console.log(error))
        },
        methods: {
            // randomCountry() {
            //     this.countriesLength = this.countries.length
            //     console.log("The length of the array is " + this.countriesLength)
            //     this.randomCountryNum = (Math.round(Math.random()*this.countriesLength))
            //      console.log("The number of the random country is  " + this.randomCountryNum)
            //     return 


            // }
        }
    }
</script>

<style>

</style>
