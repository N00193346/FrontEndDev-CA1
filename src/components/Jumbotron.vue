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

<style scoped>
.jumbo {
  background-color: aliceblue;
  padding: 30px;
  margin: 25px;
  width: 1080px;
  height: 600px;
  /* left:0;
    right:0; */
  /* border: solid 2px; */
  background-repeat: no-repeat;
  background-size: 100%;
  display: flex;
  /* align-content: center; */
  flex-direction: column;
}

.jumboTitle {
  color: white;
  font-size: 52px;
  font-weight: 600;
  font-style: italic;
  text-shadow: #000 0px 0px 5px;
}
.jumboText {
  color: white;
  font-size: 24px;
  text-shadow: #000 0px 0px 10px;
}
</style>
