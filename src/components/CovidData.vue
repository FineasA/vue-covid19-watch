<template>
  <div class="container">
    <div class="row justify-content-center">
      <b-card
        overlay
        img-src="https://images.unsplash.com/photo-1583324113626-70df0f4deaab?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60"
        img-alt="Card Image"
        text-variant="white"
        title="COVID-19 Information"
        sub-title="All information is up to date and accurate."
      >
        <b-card-text style="font-size: 23px; text-align:center;">
          There have been a total of
          <span style="font-size: 35px; color: #F3F712; font-weight: bold">{{
            virus_data.confirmed
          }}</span>
          confirmed cases of
          <span style="color:red; font-weight: bold">COVID-19</span>
        </b-card-text>

        <b-card-text style="font-size: 23px; text-align:center;">
          There have been a total of
          <span style="font-size: 35px; color: #12F750; font-weight: bold">{{
            virus_data.recovered
          }}</span>
          recovered cases of
          <span style="color:red; font-weight: bold">COVID-19</span>
        </b-card-text>

        <b-card-text style="font-size: 23px; text-align:center;">
          There have been a total of
          <span style="font-size: 35px; color: #FFC300; font-weight: bold">{{
            virus_data.deaths
          }}</span>
          death cases from
          <span style="color:red; font-weight: bold">COVID-19</span>
        </b-card-text>
      </b-card>
    </div>
    <hr />
    <div class="row justify-content-center">
      <input
        @keyup.enter="countryFilter"
        class="user-input-search"
        type="text"
        v-model="searchQuery"
        placeholder="Search by country..."
      />
    </div>
    <br />
    <br />
    <div class="row">
      <b-table striped hover :items="virus_data.confirmedDetails"></b-table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      untouchedData: {
        confirmed: 0,
        recovered: 0,
        deaths: 0,
        confirmedDetails: []
      },
      virus_data: {
        confirmed: 0,
        recovered: 0,
        deaths: 0,
        confirmedDetails: []
      },
      raw: [],
      slide: 0,
      sliding: null,
      allowed: [
        "provinceState",
        "countryRegion",
        "confirmed",
        "deaths",
        "recovered"
      ],
      searchQuery: "",
      filtered: false
    };
  },
  methods: {
    getData() {
      //get number statistics
      axios.get("https://covid19.mathdro.id/api").then(response => {
        this.virus_data.confirmed = response.data.confirmed.value;
        this.virus_data.recovered = response.data.recovered.value;
        this.virus_data.deaths = response.data.deaths.value;
      });
      //get confirmed virus country details
      axios.get("https://covid19.mathdro.id/api/confirmed").then(response => {
        this.untouchedData = response.data;
        this.raw = response.data;
        //filter unwanted data
        this.raw = this.raw.filter(el => {
          this.virus_data.confirmedDetails.push({
            provinceState: el.provinceState,
            countryRegion: el.countryRegion,
            confirmed: el.confirmed,
            deaths: el.deaths,
            recovered: el.recovered
          });
        });
      });
    },
    countryFilter() {
      if (this.filtered) {
        this.virus_data.confirmedDetails = this.untouchedData.confirmedDetails;
        this.filtered = false;
        return;
      }
      let filtered = this.virus_data.confirmedDetails.filter(el => {
        return el.countryRegion === this.searchQuery;
      });
      this.virus_data.confirmedDetails = filtered;
      this.filtered = true;
      console.log(this.untouchedData);
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

<style>
img {
  opacity: 0.83;
}
.user-input-search:focus {
  outline: none;
}
</style>
