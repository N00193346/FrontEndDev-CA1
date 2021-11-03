<template>
  <div class="container">
    <!-- If the length of the array is greater than zero, show the countries related to the term -->
    <div v-if="this.countries.length > 0">
      <CountryCard
        v-for="country in countries"
        :key="country.ccn3"
        :country="country"
      />
    </div>
    <!-- Else display -->
    <div v-else>
      <b-alert show variant="danger" class="margin">
        <div class="errorTitle">Search Error</div>
        <div class="errorSubtitle">
          Unfortunately, "{{ this.$route.params.country }}" does not relate to
          any country in our Database
        </div>
      </b-alert>

      <div class="title">
        Please try again, or click
        <router-link :to="{ name: 'all_countries' }">here</router-link> to see
        all countries
      </div>
      <b-form-input
        v-model="newTerm"
        placeholder="Search for a country"
        v-on:keyup.enter="searchNewCountry()"
      ></b-form-input>
      <b-button
        size="md"
        class="margin"
        variant="primary"
        type="submit"
        @click="searchNewCountry()"
        >Search</b-button
      >
      <!-- <div class="mt-2">Value: {{ newTerm }}</div> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CountryCard from "@/components/CountryCard";
// import EventCard from '@/components/EventCard'
const EVENTS_API_KEY = "&apikey=EcGMSU6HjGfoAlCxMfW8P70iTXUrz8Le";
const EVENTS_URL =
  "https://app.ticketmaster.com/discovery/v2/events.json?countryCode=";

export default {
  name: "Country",
  components: {
    CountryCard,
    // EventCard,
  },
  data() {
    return {
      countries: [],
      events: [],
      newTerm: "",
    };
  },
  mounted() {
    axios
      .get(`https://restcountries.com/v3.1/name/${this.$route.params.country}`)
      .then((response) => {
        console.log(response.data);
        this.countries = response.data;
        this.getEventData(this.countries[0].cca2);
      })
      .catch(function(error) {
        console.log(error);
        // alert("No country with that name exists")
        //  this.$router.push('/');
      });
  },
  methods: {
    getEventData(countryCode) {
      axios
        .get(`${EVENTS_URL}${countryCode}${EVENTS_API_KEY}`)
        .then((response) => {
          console.log(response.data._embedded.events);
          this.events = response.data._embedded.events;
        })
        .catch((error) => console.log(error));
    },
    searchNewCountry() {
      if (!this.newTerm) {
        alert("Please enter a search term");
        return;
      }

      this.$router.push(`/searchCountry/${this.newTerm}`);
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  align-content: flex-start;
  flex-wrap: wrap;
}

.margin {
  margin-top: 20px;
}
.errorTitle {
  font-size: 44px;
  text-align: center;
}

.errorSubtitle {
  font-size: 32px;
  text-align: center;
}

.errorContainer {
  background-color: aqua;
  border: solid 2px;
}

.title {
  font-size: 24px;
}

.margin {
  margin: 20px;
}
</style>
