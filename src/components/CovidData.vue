<template>
  <div class="container">
    <b-carousel
      id="carousel-fade"
      v-model="slide"
      :interval="4000"
      fade
      indicators
      background="#ababab"
      img-width="1024"
      img-height="480"
      style="text-shadow: 1px 1px 2px #333;"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <!-- Text slides with image -->
      <b-carousel-slide
        img-width="1024"
        img-height="480"
        caption="Confirmed Cases"
        text="Total number of cases that have been confirmed"
        img-src="https://images.unsplash.com/photo-1583324113626-70df0f4deaab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2089&q=80"
        >{{ this.virus_data.confirmed }}</b-carousel-slide
      >

      <!-- Slides with custom text -->
      <b-carousel-slide
        img-width="1024"
        img-height="480"
        caption="Total Recovered"
        text="Number of people who have recovered"
        img-src="https://images.unsplash.com/photo-1516574187841-cb9cc2ca948b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1650&q=80"
      >
        {{ this.virus_data.recovered }}
      </b-carousel-slide>

      <!-- Slides with image only -->
      <b-carousel-slide
        img-width="1024"
        img-height="480"
        caption="Total Deaths"
        text="Number of deaths reported"
        img-src="https://images.unsplash.com/photo-1538108149393-fbbd81895907?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1700&q=80s"
        >{{ this.virus_data.deaths }}</b-carousel-slide
      >
    </b-carousel>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      virus_data: {
        confirmed: 0,
        recovered: 0,
        deaths: 0
      },
      slide: 0,
      sliding: null
    };
  },
  methods: {
    getData() {
      axios.get("https://covid19.mathdro.id/api").then(response => {
        console.log(response);
        this.virus_data.confirmed = response.data.confirmed.value;
        this.virus_data.recovered = response.data.recovered.value;
        this.virus_data.deaths = response.data.deaths.value;
      });
    },
    onSlideStart() {
      this.sliding = true;
    },
    onSlideEnd() {
      this.sliding = false;
    }
  },
  created() {
    this.getData();
  }
};
</script>

<style></style>
