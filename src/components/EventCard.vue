<template>

 

    <b-card
    :title= event.name
     :img-src= event.images[0].url
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem;"
    class="mb-2 eventCard"
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

<style  scoped>
.card{
  margin: 20px;

}

.eventCard > img{
   object-fit: cover;
  height: 150px;
  width: 330px;
}
</style>
