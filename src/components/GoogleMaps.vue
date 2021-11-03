<template>
  <div>
    <iframe
      width="100%"
      height="450"
      style="border:0"
      loading="lazy"
      allowfullscreen
      :src="
        `https://www.google.com/maps/embed/v1/place?key=AIzaSyDuFmToFBLDcb07oNdj66Gvebao37XTG74&q=${countryName}`
      "
    >
    </iframe>
  </div>
</template>

<script>
export default {
  name: "GoogleMaps",
  props: {
    countryName: [],
  },
};
</script>

<style scoped></style>
