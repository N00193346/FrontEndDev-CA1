<template>
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
</template>
     

<script>

import axios from 'axios'
// import CountryCard from '@/components/CountryCard'
const UNSPLASH_URL = "https://api.unsplash.com/search/photos/?client_id="
const UNSPLASH_API_KEY = "XhqXA2Jig1drfBj96ploqpKdat9N94vn0GPzbrYjwK8&"


    export default {
        name: 'AllCountries',
        components: {
            // CountryCard,
        },
        data () {
            
            return {
                countriesLength: null,
                randomCountryNum: null,
                countries: [],
                countryImage: String,
               
            }
        },
        mounted () {
            axios.get('https://restcountries.com/v3.1/all')
                 .then(response => {
                     console.log(response)
                     this.countries = response.data
                     this.countriesLength = this.countries.length
                     console.log("The length of the array is " + this.countriesLength)
                     this.randomCountryNum = (Math.round(Math.random()*this.countriesLength))
                     console.log("The number of the random country is  " + this.randomCountryNum)
                     this.countries = response.data[this.randomCountryNum]
                    console.log("The name of the random country is  " + this.countries.name.common)
                    this.getCountryImage(this.countries.name.common)
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
