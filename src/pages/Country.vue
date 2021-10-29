<template>
  <div>
   
    <b-container fluid="md">
      <b-row>
        <div class="title">
     {{ this.countries[0].name.official }} 
        </div>
      </b-row>

      <b-row>
      <b-carousel
        controls
        :interval="0"
        indicators
        no-animation
        img-width="100vh"
        img-height="480"
      >
        <b-carousel-slide :img-src="this.countryImages[0]">
 
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[1]">
   
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[2]">
      
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[3]">
     
        </b-carousel-slide>
        <b-carousel-slide :img-src="this.countryImages[4]">
      
        </b-carousel-slide>
      </b-carousel>
      </b-row>

      <b-row>
        <b-col cols="8">
            <div class="subtitle">
              Location:
            </div>
          <iframe
           
            width="100%"
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
             <div class="subtitle">
              Weather:
            </div>
            <b-card 
              :img-src="`http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@2x.png`"
              img-alt="Image"
              img-height="100"
              img-width="100"
              img-top
              tag="article"
           
              class="mb-2"
  >           
               <b-card-text class="cardText">
                {{ this.countries[0].capital[0]  }}
              </b-card-text>
              <b-card-text class="cardText">
                {{ Math.round(this.weather.main.temp) }}°C
              </b-card-text>
               <b-card-text class="cardText">
                {{this.weather.weather[0].main}}
              </b-card-text>
            </b-card>
        </b-col>
      </b-row>

      <b-row>
         <div class="subtitle">
              Public Holidays:
            </div>
         <b-card
          :title = this.holidays[0].name
          :img-src= this.holidayImages[0]
          img-alt="Image"
          img-top
          tag="article"
          style="max-width: 20rem;"
          class="mb-2"
        >
          <b-card-text>
            {{this.holidays[0].description}}
          </b-card-text>

         
  </b-card>

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
const CALANDARIFIC_URL = "https://calendarific.com/api/v2/holidays?&";
const CALANDARIFIC_API_KEY = "api_key=a14b28bae234cb619999d15226d47cb2244b9992";

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
      holidays: [],
      holidayImages: [],
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
        this.getCountryHolidays(this.countries[0].cca2);
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
    //Get country's weather using latitude and longitude 
    getCountryWeather(lat, lon) {
      axios
        .get(
          `${OPENWEATHER_URL}lat=${lat}&lon=${lon}${OPENWEATHER_API_KEY}&units=metric`
        )
        .then((response) => {
          this.weather = response.data;
          console.log("The weather icon is : " + this.weather.weather[0].icon)
        })

        .catch((error) => console.log(error));
    },
    //Get country's holidays using country code
     getCountryHolidays(countryCode) {
      axios
        .get(`${CALANDARIFIC_URL}${CALANDARIFIC_API_KEY}&country=${countryCode}&year=2021`)
        .then((response) => {
          console.log(response.data);
          //Loop through results to get 4 holidays
           for (var i = 0; i < 4; i++) {
            //Push the name of the holiday into the holidays array
            this.holidays.push(response.data.response.holidays[i]);
            //Pass the name of holiday into the get holiday image to get a related image
            this.getHolidayImages(response.data.response.holidays[i].name);
          }
          console.log("The holidays are: " + this.holidays)
        })
        .catch((error) => console.log(error));
    }, 
    //Get images related to holiday
    getHolidayImages(holidayName) {
      axios
        .get(`${UNSPLASH_URL}${UNSPLASH_API_KEY}&query=${holidayName}`)
        .then((response) => {
          console.log(response);
          this.holidayImages.push(response.data.results[0].urls.regular);
          console.log("The Holiday Image urls " + this.holidayImages);
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

.title {
  margin-top: 20px;
  margin-bottom: 20px;
  font-size: 64px;
  font-weight: 700;
}

.subtitle {
  margin-bottom: 10px;
  font-size: 46px;
  font-weight: 700;
}

/* .image {
  display: block;
  margin-left: auto;
  margin-right: auto;
  max-width: 100%;
  max-height: 480px;
} */
.carousel-item img {
  height: 80vh !important ;
  width: 124vh !important ;
}
.carouselTitle {
  font-size: 52px;
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;
  margin-bottom: 600px;
  margin-right: 350px;
}

.flex{
  display: flex;
}

.margin {
  margin-top: 20px;
}

.cardText{
  font-size: 32px;
  font-weight: 700;
}
</style>
