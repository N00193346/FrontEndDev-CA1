<template>
  <div>
  <b-navbar class="navbar" toggleable="lg" type="dark" variant="dark">
    <b-navbar-brand href="#">Country Thing</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item><router-link style="text-decoration: none; color: inherit;" :to="{name: 'home'}">Home</router-link></b-nav-item>
        <b-nav-item><router-link style="text-decoration: none; color: inherit;" :to="{name: 'all_countries'}">All</router-link></b-nav-item>
      </b-navbar-nav>
    </b-collapse>

   
      <b-navbar-nav class="ml-auto">
        <b-nav-form class="input-group"> 
          <b-form-input size="sm"  class="mr-sm-2" placeholder="Search" v-model="term" v-on:keyup.enter="searchCountry()"></b-form-input>
        </b-nav-form>
         <b-button size="sm"   class="my-2 my-sm-0 marginLeft"  type="submit" @click="searchCountry()">Search</b-button>
   </b-navbar-nav>
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
             
               this.$router.push(`/searchCountry/${this.term}`)
          }
        }
    }
</script>

<style>
.navbar {
    margin: 0;
    padding: 7.5px 15px;
}

.marginLeft{
  margin-left: 10px;
}
</style>
