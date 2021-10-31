<template>

 
    <b-card
    :title= holiday.name
    :img-src= holidayImage
    img-alt="Image"
    tag="article"
    style="max-width: 20rem;"
    class="mb-2"
  >

    <b-card-text>
    <b>Date: </b>{{ holiday.date.datetime.day}}/{{ holiday.date.datetime.month}}
    <br>
     {{ holiday.description}}
    </b-card-text>

  </b-card>


 

</template>

<script>
    export default {
        name: 'HolidayCard',
        props: {
            holiday: Object,
            holidayImage: Object
        }
    }
</script>

<style>
.card{
  margin: 20px;
}

img {
    width: 100%;
    height: 15vw;
    object-fit: cover;
}
</style>
