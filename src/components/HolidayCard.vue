<template>

 
    <b-card
    :title= holiday.name
    :img-src= holidayImage
    img-alt="Image"
    tag="article"
    style="max-width: 20rem;"
    class="mb-2 holidayCard shadow p-3 mb-5 bg-white rounded"
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



.holidayCard{
  margin: 20px;
  width: 100%;

}

.holidayCard > img{
 
  object-fit: cover;
  height: 300px;
  width: 285px;
}
</style>
