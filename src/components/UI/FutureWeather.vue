<template>
  <div id="forecast" class="">
    <div id="forecast-container" class="flex sm:gap-12 gap-3">
      <div
        v-for="(day, index) in forecastData"
        :key="index"
        id="forecast-day"
        class="flex flex-col items-center"
      >
        <div class="lg:whitespace-nowrap text-center">
          {{ formatDate(day.date) }}
        </div>
        <img :src="day.day.condition.icon" alt="icon" />
        <div>{{ day.day.maxtemp_c }}°C</div>
        <div>{{ day.day.condition.text }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["latitude", "longitude", "place"],
  data() {
    return {
      forecastData: [],
    };
  },
  mounted() {
    this.getForecastData(this.latitude, this.longitude, this.place);
  },
  methods: {
    getForecastData(lat, lon, place) {
      const query = place ? place : `${lat},${lon}`;
      console.log("future weather place", place);
      axios
        .get(`https://api.weatherapi.com/v1/forecast.json`, {
          params: {
            key: "d72c5a037a5745808e6141937240609",
            q: query,
            days: 3,
          },
        })
        .then((response) => {
          this.forecastData = response.data.forecast.forecastday;
          //console.log(this.forecastData);
        })
        .catch((error) => {
          console.error("API forecast request failed:", error);
        });
    },
    formatDate(dateStr) {
      const date = new Date(dateStr);
      return date.toLocaleDateString("tr-TR", {
        weekday: "long",
        day: "numeric",
        month: "long",
      });
    },
  },
};
</script>
