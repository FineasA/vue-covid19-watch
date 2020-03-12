<template>
  <div class="container">
    <div class="row justify-content-center">
      <h2 class="animated fadeIn">Covid19 Data Display</h2>
    </div>
    <hr />
    <div class="row">
      <covid-data></covid-data>
    </div>
  </div>
</template>

<script>
import CovidData from "./components/CovidData.vue";

export default {
  components: {
    CovidData
  }
};
</script>

<style></style>
