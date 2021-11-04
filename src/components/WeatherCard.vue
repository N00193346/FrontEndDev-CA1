<template>
  <b-card
    :img-src="
      `http://openweathermap.org/img/wn/${weatherInfo.weather[0].icon}@2x.png`
    "
    img-alt="Image"
    img-height="245"
    img-width="245"
    img-top
    tag="article"
    class="mb-2"
  >
    <b-card-text class="cardText">
      {{ country.capital[0] }}
    </b-card-text>
    <b-card-text class="cardText">
      {{ Math.round(weatherInfo.main.temp) }}°C
    </b-card-text>
    <b-card-text class="cardText">
      {{ weatherInfo.weather[0].main }}
    </b-card-text>
  </b-card>
</template>

<script>
export default {
  name: "WeatherCard",
  props: {
    weatherInfo: Object,
    country: Object,
  },
};
</script>

<style scoped>
.cardText {
  font-size: 32px;
  font-weight: 700;
}
</style>
