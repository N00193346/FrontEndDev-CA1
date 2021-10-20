<template>
    <div>

    <div class="search-box">
        <input type="text" v-model="term" v-on:keyup.enter="searchCountries()"/>
        <b-button class="buttonMargin float-end" variant="primary" @click="searchCountries()">Search</b-button>
    </div>

    <!-- <b-row>
    <b-col>
    <b-card v-if=searchCountries()
    :title= country.name.common 
    :img-src= country.flags.png
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem;"
    class="mb-2"
  >
    <b-card-text>
     {{ country.capital[0]}}
    </b-card-text>

    <b-button href="#" variant="primary">View Country</b-button>
  </b-card>
    </b-col>

  </b-row> -->

    </div>
</template>
<script>
import axios from 'axios';
export default {
    name: 'SearchCountry',
    data() {
        return {
            countries: [],
            term: "",

        };
    },
    mounted() {
      
    },

    methods: {
        searchCountries() {
            if(!this.term) {
                alert("Please enter a search term")
                return
                }
               axios.get(`https://restcountries.com/v3.1/name/${this.term}`)
                    .then((response) => {
                    console.log(response.data)
                     this.countries = response.data;
                 })
                    .catch(error => console.log(error))
        }, 

    },
    }

    

</script>
<style>
</style>
