<template>
  <div class="flex flex-col lg:flex-col gap-4 w-[80%]">
    <search-bar @placeUpdated="updateSearchedPlace"></search-bar>
    <weather-card
      v-if="searchedPlace || (lat && long)"
      :place="searchedPlace"
      :lat="lat"
      :long="long"
      :message="message"
    ></weather-card>
    <OtherWeatherCard></OtherWeatherCard>
  </div>
</template>

<script>
import { ref, provide } from "vue";
import WeatherCard from "./components/WeatherCard.vue";
import OtherWeatherCard from "./components/UI/OtherWeatherCard.vue";
import SearchBar from "./components/UI/SearchBar.vue";

export default {
  name: "App",
  components: {
    WeatherCard,
    OtherWeatherCard,
    SearchBar,
  },
  data() {
    return {
      searchedPlace: null,
      message: null,
    };
  },
  setup() {
    const lat = ref(null);
    const long = ref(null);

    const getCurrentLocation = () => {
      navigator.geolocation.getCurrentPosition((position) => {
        lat.value = position.coords.latitude;
        long.value = position.coords.longitude;
      });
    };
    provide("latitude", lat);
    provide("longtitude", long);
    getCurrentLocation();
    return { lat, long };
  },
  methods: {
    updateSearchedPlace(place) {
      if (place == null || place == "") {
        this.message = "Please dont leave the place blank ! Search again.";
      } else {
        this.searchedPlace = place;
        (this.lat = null), (this.long = null);
        this.message = null;
      }
    },
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&display=swap");
#app {
  background-image: url("./assets/weatherAppBackground.jpg");
  display: flex;
  background-size: cover;
  justify-content: center;
  font-family: "Open Sans", sans-serif;
  font-optical-sizing: auto;

  padding: 10px;
  background-repeat: no-repeat;
  background-position: center;
}
@media screen and (min-width: 1024px) {
  #app {
    height: 100vh;
    align-items: center;
  }
}
</style>
