<template>
  <div>
    <router-link
      style="text-decoration: none; color: inherit;"
      :to="{
        name: 'country',
        params: { country: country.name.common },
      }"
    >
      <b-jumbotron
        :style="`background-image: url(${countryImage})`"
        class="jumbo"
      >
        <div class="jumboTitle">
          <p>{{ country.name.official }}</p>
        </div>
        <div class="jumboText">
          <p>Most commonly know as {{ country.name.common }}</p>
        </div>
        <div class="jumboText">
          <p>From the {{ country.subregion }} region</p>
        </div>
      </b-jumbotron>
    </router-link>
  </div>
</template>

<script>
export default {
  name: "CountryViewer",
  props: {
    country: Object,
    countryImage: Object,
  },
};
</script>

<style scoped></style>
