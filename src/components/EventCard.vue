<template>

 

    <b-card
    :title= event.name
     :img-src= event.images[0].url
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem;"
    class="mb-2"
  >

  </b-card>


 

</template>

<script>
    export default {
        name: 'EventCard',
        props: {
            event: Object
        }
    }
</script>

<style>
.card{
  margin: 20px;

}
</style>
