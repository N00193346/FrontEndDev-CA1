<template>
  <div>
    <b-container>
      <b-carousel
        controls
        :interval="0"
        indicators
        no-animation
        img-width="1080"
        img-height="480"
      >
        <b-carousel-slide :img-src="this.countryImages[0]">
          <h1 class="carouselTitle">{{ this.countries[0].name.official }}</h1>
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[1]">
          <h1 class="carouselTitle">{{ this.countries[0].name.official }}</h1>
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[2]">
          <h1 class="carouselTitle">{{ this.countries[0].name.official }}</h1>
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[3]">
          <h1 class="carouselTitle">{{ this.countries[0].name.official }}</h1>
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[4]">
          <h1 class="carouselTitle">{{ this.countries[0].name.official }}</h1>
        </b-carousel-slide>
      </b-carousel>
    </b-container>

    <b-container class="bv-example-row">
      <b-row>
        <b-col>
          <iframe
            class="margin"
            width="600"
            height="450"
            style="border:0"
            loading="lazy"
            allowfullscreen
            :src="
              `https://www.google.com/maps/embed/v1/place?key=AIzaSyDuFmToFBLDcb07oNdj66Gvebao37XTG74&q=${this.countries[0].name.common}`
            "
          >
          </iframe>
        </b-col>
        <b-col>
          <h1>
            The weather is {{ Math.round(this.weather.main.temp) }}°c in
            {{ this.countries[0].capital[0] }}
          </h1>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
// import CountryCard from '@/components/CountryCard'
// import EventCard from '@/components/EventCard'
const EVENTS_API_KEY = "&apikey=EcGMSU6HjGfoAlCxMfW8P70iTXUrz8Le";
const EVENTS_URL =
  "https://app.ticketmaster.com/discovery/v2/events.json?countryCode=";
const UNSPLASH_URL = "https://api.unsplash.com/search/photos/?client_id=";
const UNSPLASH_API_KEY = "XhqXA2Jig1drfBj96ploqpKdat9N94vn0GPzbrYjwK8&";
const OPENWEATHER_URL = "https://api.openweathermap.org/data/2.5/weather?";
const OPENWEATHER_API_KEY = "&appid=3cc88d355d79a4f5c037a56d50e686b2";
// const GOOGLEMAPS = "https://www.google.com/maps/embed/v1/place?key=AIzaSyDuFmToFBLDcb07oNdj66Gvebao37XTG74&q="

export default {
  name: "Country",
  components: {
    // CountryCard,
    // EventCard,
    // GOOGLEMAPS,
  },
  data() {
    return {
      countries: [],
      events: [],
      countryImages: [],
      weather: [],
      lat: String,
      lon: String,
    };
  },
  mounted() {
    //Get the countrys name from the url, search that country's information
    axios
      .get(`https://restcountries.com/v3.1/name/${this.$route.params.country}`)
      .then((response) => {
        console.log(response.data);
        //Put country's information in countries array
        this.countries = response.data;

        this.getEventData(this.countries[0].cca2);
        //Put country name into get images method
        this.getCountryImages(this.countries[0].name.common);
        //Put the latitude and longitude into the weather method
        this.lat = this.countries[0].capitalInfo.latlng[0];
        this.lon = this.countries[0].capitalInfo.latlng[1];
        this.getCountryWeather(this.lat, this.lon);
      })
      .catch((error) => console.log(error));
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
    //Get images related to country
    getCountryImages(countryName) {
      axios
        .get(`${UNSPLASH_URL}${UNSPLASH_API_KEY}&query=${countryName}`)
        .then((response) => {
          console.log(response);
          for (var i = 0; i < 5; i++) {
            this.countryImages.push(response.data.results[i].urls.regular);
          }
          console.log("The Image urls " + this.countryImages);
        })

        .catch((error) => console.log(error));
    },
    getCountryWeather(lat, lon) {
      axios
        .get(
          `${OPENWEATHER_URL}lat=${lat}&lon=${lon}${OPENWEATHER_API_KEY}&units=metric`
        )
        .then((response) => {
          this.weather = response.data;
        })

        .catch((error) => console.log(error));
    },
  },
};
</script>

<style>
.container {
  display: flex;
  align-content: flex-start;
  flex-wrap: wrap;
}

.image {
  display: block;
  margin-left: auto;
  margin-right: auto;
  max-width: 1024px;
  max-height: 480px;
}
.carousel-item img {
  height: 80vh !important ;
  width: 100vh !important ;
}
.carouselTitle {
  font-size: 52px;
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;
  margin-bottom: 600px;
  margin-right: 350px;
}

.margin {
  margin-top: 20px;
}
</style>
