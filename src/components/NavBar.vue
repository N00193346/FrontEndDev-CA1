<template>
  <div>
  <b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="#">Country Thing</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item><router-link :to="{name: 'home'}">Home</router-link></b-nav-item>
        <b-nav-item><router-link :to="{name: 'all_countries'}">All</router-link></b-nav-item>
      </b-navbar-nav>
    </b-collapse>

   
    
        <b-nav-form right> 
          <b-form-input size="sm"  class="mr-sm-2" placeholder="Search" v-model="term" v-on:keyup.enter="searchCountry()"></b-form-input>
          <b-button size="sm"   class="my-2 my-sm-0"  type="submit" @click="searchCountry()">Search</b-button>
        </b-nav-form>
   
  </b-navbar>
</div>
</template>

<script>
    export default {
        name: 'NavBar',
        methods: {
          searchCountry(){
            if(!this.term) {
              alert("Please enter a search term")
              return
              }
             
               this.$router.push(`/countries/${this.term}`)
          }
        }
    }
</script>

<style>

</style>
