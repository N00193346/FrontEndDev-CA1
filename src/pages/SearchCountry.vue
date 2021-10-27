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
// import EventCard from '@/components/EventCard'
const EVENTS_API_KEY = "&apikey=EcGMSU6HjGfoAlCxMfW8P70iTXUrz8Le";
const EVENTS_URL = "https://app.ticketmaster.com/discovery/v2/events.json?countryCode="

export default {
  name: 'Country',
       components: {
            CountryCard,
            // EventCard,
        },
        data () {
            return {
                countries: [],
                events: []
            }
        },
        mounted(){
          axios.get(`https://restcountries.com/v3.1/name/${this.$route.params.country}`)
              .then(response => {
                console.log(response.data)
                this.countries = response.data
                this.getEventData(this.countries[0].cca2);
              })
              .catch(error => console.log(error))
        },
        methods: {
          getEventData(countryCode){
            axios.get(`${EVENTS_URL}${countryCode}${EVENTS_API_KEY}`)
                 .then(response => {
                 console.log(response.data._embedded.events)
                this.events = response.data._embedded.events
              })
                 .catch(error => console.log(error))
          }
        }
}
</script>

<style>
.container{
  display: flex;
  align-content: flex-start;
  flex-wrap: wrap;
}
</style>
