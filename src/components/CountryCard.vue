<template>
  <router-link
    style="text-decoration: none; color: inherit;"
    :to="{ name: 'country', params: { country: country.name.common } }"
  >
    <b-card
      fluid-grow
      :title="country.name.common"
      :img-src="country.flags.png"
      img-alt="Image"
      img-top
      tag="country"
      style="max-width: 21rem;"
      class="mb-2 countryCard shadow p-3 mb-5 bg-white rounded"
    >
      <b-card-text>
        <b>Capital City: </b>{{ country.capital[0] }}
        <br />
        <b>Region : </b> {{ country.subregion }}
        <!-- <b>Languages: </b>{{ this.languages}}
        <hr>
      <b>Currency: </b> {{this.currenciesName.toString() + " (" + this.currenciesSymbol + ")"}}
        <hr>
       <b>Population: </b>  {{ country.population}} -->
      </b-card-text>
    </b-card>
  </router-link>
</template>

<script>
export default {
  name: "CountryCard",
  props: {
    country: Object,
  },
  data() {
    return {
      languages: [],
      currencies: [],
      currenciesName: [],
      currenciesSymbol: [],
    };
  },
  mounted() {
    //Get object values from languages, turn them into a string
    this.languages = Object.values(this.country.languages);
    this.languages = this.languages.toString();

    //Get object values from currency
    this.currencies = Object.values(this.country.currencies);
    //Loop through currencies, push the name and symbol into respective array
    for (var i = 0; i < this.currencies.length; i++) {
      this.currenciesName.push(this.currencies[i].name);
      this.currenciesSymbol.push(this.currencies[i].symbol);
    }
  },
};
</script>

<style scoped>
.card {
  margin: 20px;
}

.countryCard {
  object-fit: contain;
}

.countryCard > img {
  object-fit: cover;
  height: 150px;
  width: 300px;
}
</style>
