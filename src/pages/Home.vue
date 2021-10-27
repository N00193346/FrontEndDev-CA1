<template>
<div>
<b-container>
 
<div>
  <b-jumbotron class="jumbo"  :header="`${this.countries.name.official}`" :lead="`Most commonly know as ${this.countries.name.common}`">
    <p><b>Region: </b> {{this.countries.subregion}}</p>
    <img class="image" :src="this.countryImage">

    
  </b-jumbotron>
</div>
<div class="title">
<h1> More countries from  {{this.countries.subregion}}</h1>
</div>
</b-container>

  <div class="container">
    <CountryCard 
      v-for="country in relatedCountries"
      :key = "country.ccn3"
      :country="country"/>
  </div>
</div>
</template>
     

<script>

import axios from 'axios'
import CountryCard from '@/components/CountryCard'
const UNSPLASH_URL = "https://api.unsplash.com/search/photos/?client_id="
const UNSPLASH_API_KEY = "XhqXA2Jig1drfBj96ploqpKdat9N94vn0GPzbrYjwK8&"
const RESTCOUNTRIES_URL = "https://restcountries.com/v3.1/"


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
                countryImage: String,
                relatedCountries: [],
               
            }
        },
        mounted () {
            axios.get(`${RESTCOUNTRIES_URL}all`)
                 .then(response => {
                     console.log(response)
                     //Get all countries
                     this.countries = response.data
                     //Get the length of the number of countries in the array
                     this.countriesLength = this.countries.length
                     console.log("The length of the array is " + this.countriesLength)
                     //Get a random number between 0 and the length of the countries array
                     this.randomCountryNum = (Math.round(Math.random()*this.countriesLength))
                     console.log("The number of the random country is  " + this.randomCountryNum)
                     //Assign the data of the country with the random number into the countries array
                     this.countries = response.data[this.randomCountryNum]
                    console.log("The name of the random country is  " + this.countries.name.common)
                    //Passing the name of the country into a method to get a related image for that country
                    this.getCountryImage(this.countries.name.common)
                    //Passing the country's subregion to get countries from the same region
                    this.getCountryBySubregion(this.countries.subregion)
                     })
                 .catch(error => console.log(error))
        },
        methods: {
            getCountryImage(countryName) {
                  axios.get(`${UNSPLASH_URL}${UNSPLASH_API_KEY}&query=${countryName}`)
                   .then(response => {
                     console.log(response)
                     this.countryImage = response.data.results[0].urls.regular
                    console.log("Image Url is:" + this.countryImage)
                     })
                 .catch(error => console.log(error))
            },
            getCountryBySubregion(subregion){
            axios.get(`${RESTCOUNTRIES_URL}region/${subregion}`)
                 .then(response => {
                     console.log(response)
                     //Looping so only get 3 related countries
                      for(var i=0; i<3; i++){
                        this.relatedCountries.push(response.data[i])
                    }
                     })
                 .catch(error => console.log(error))
            }
       
        }
    }
</script>

<style>
.container {

    margin-top: 20px;
    display: flex;
    justify-content: center;
}

.jumbo{
    background-color: aliceblue;
    padding: 30px;
    margin: 25px;
    left:0;
    right:0;
    border: solid 2px;
    display: flex;
    align-content: center;
    flex-direction: column;
}

.image{
     display: block;
  margin-left: auto;
  margin-right: auto;
    max-width: 1000px;
     max-height: 1000px;
    
}
</style>
