<template>
  <div>
    <b-container fluid="lg">
      <div v-if="this.countries.length > 0">
        <b-row>
          <div class="title">
            {{ this.countries[0].name.official }}
          </div>
          <div class="countryInfo">
            <b>Capital City: </b>{{ this.countries[0].capital[0] }}
            <br />
            <b>Languages: </b>{{ this.languages }}
            <br />
            <b>Currency: </b>
            {{
              this.currenciesName.toString() +
                " (" +
                this.currenciesSymbol +
                ")"
            }}
            <br />
            <b>Population: </b> {{ this.countries[0].population }}
          </div>
        </b-row>

        <b-row>
          <!-- If there are images -->
          <div v-if="this.countryImages">
            <ImageCarousel :images="this.countryImages" />
            <!-- Close if no images -->
          </div>
          <div v-else>
            <b-alert show variant="danger"
              ><a href="#" class="alert-link"
                >Unable to find related images</a
              ></b-alert
            >
          </div>
        </b-row>

        <b-row class="margin">
          <b-col cols="8">
            <div class="subtitle">
              Location:
            </div>
            <GoogleMaps :countryName="this.countries[0].name.common" />
          </b-col>

          <b-col>
            <div class="subtitle">
              Weather:
            </div>

            <WeatherCard
              :weatherInfo="this.weather"
              :country="this.countries[0]"
            />
          </b-col>
        </b-row>

        <b-row class="margin">
          <div class="subtitle">
            Public Holidays:
          </div>
        </b-row>
        <b-row>
          <!-- If there is data in the holiday array -->
          <div v-if="this.holidays">
            <div class="cardContainer">
              <HolidayCard
                v-for="(holiday, index) in holidays"
                :key="holiday"
                :holiday="holiday"
                :holidayImage="holidayImages[index]"
              />
            </div>
            <!-- Closing holiday if -->
          </div>
          <div v-else>
            <b-alert show variant="danger"
              ><a href="#" class="alert-link"
                >Unable to find holiday information</a
              ></b-alert
            >
          </div>
        </b-row>
        <!-- Closing country if -->
      </div>
      <div v-else>
        <b-alert show variant="danger"
          ><a href="#" class="alert-link"
            >Unable to find {{ route.params }}</a
          ></b-alert
        >
      </div>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
import HolidayCard from "@/components/HolidayCard";
import ImageCarousel from "@/components/ImageCarousel";
import GoogleMaps from "@/components/GoogleMaps";
import WeatherCard from "@/components/WeatherCard";
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
    HolidayCard,
    ImageCarousel,
    GoogleMaps,
    WeatherCard,
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
      languages: [],
      currencies: [],
      currenciesName: [],
      currenciesSymbol: [],
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
        this.getCountryLanguages(this.countries[0]);
        this.getCountryCurrencies(this.countries[0]);
      })
      .catch((error) => console.log(error));
  },
  methods: {
    getCountryLanguages(country) {
      //Get object values from languages, turn them into a string
      this.languages = Object.values(country.languages);
      this.languages = this.languages.toString();
    },
    getCountryCurrencies(country) {
      //Get object values from currency
      this.currencies = Object.values(country.currencies);
      //Loop through currencies, push the name and symbol into respective array
      for (var i = 0; i < this.currencies.length; i++) {
        this.currenciesName.push(this.currencies[i].name);
        this.currenciesSymbol.push(this.currencies[i].symbol);
      }
    },
    //Get Events in country
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
          console.log("The weather icon is : " + this.weather.weather[0].icon);
        })

        .catch((error) => console.log(error));
    },
    //Get country's holidays using country code
    getCountryHolidays(countryCode) {
      axios
        .get(
          `${CALANDARIFIC_URL}${CALANDARIFIC_API_KEY}&country=${countryCode}&year=2021`
        )
        .then((response) => {
          console.log(response.data);
          //Loop through results to get 4 holidays
          for (var i = 0; i < 3; i++) {
            //Push the name of the holiday into the holidays array
            this.holidays.push(response.data.response.holidays[i]);
            //Pass the name of holiday into the get holiday image to get a related image
            this.getHolidayImages(response.data.response.holidays[i].name);
          }
          console.log("The holidays are: " + this.holidays);
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

<style scoped>
.container {
  display: flex;
  align-content: flex-start;
  flex-wrap: wrap;
}

.cardContainer {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: 100%;
}

.title {
  margin-top: 20px;
  font-size: 64px;
  font-weight: 700;
}

.subtitle {
  margin-bottom: 10px;
  font-size: 46px;
  font-weight: 700;
}

.countryInfo {
  margin-bottom: 20px;
  font-size: 24px;
}

.flex {
  display: flex;
}

.margin {
  margin-top: 20px;
}

.cardText {
  font-size: 32px;
  font-weight: 700;
}
</style>
