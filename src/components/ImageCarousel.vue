<template>
  <div>
    <b-carousel
      controls
      :interval="0"
      indicators
      no-animation
      img-width="100vh"
      img-height="480"
    >
      <b-carousel-slide :img-src="images[0]"> </b-carousel-slide>
      <b-carousel-slide :img-src="images[1]"> </b-carousel-slide>
      <b-carousel-slide :img-src="images[2]"> </b-carousel-slide>
      <b-carousel-slide :img-src="images[3]"> </b-carousel-slide>
      <b-carousel-slide :img-src="images[4]"> </b-carousel-slide>
    </b-carousel>
  </div>
</template>

<script>
export default {
  name: "ImageCarousel",
  props: {
    images: [],
  },
};
</script>

<style scoped>
.carousel-item img {
  height: 80vh !important ;
  width: 124vh !important ;
}
</style>
