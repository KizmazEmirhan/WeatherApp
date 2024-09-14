<template>
  <div class="flex flex-col lg:flex-col gap-4 w-[80%]">
    <weather-card v-if="lat && long"></weather-card>
    <OtherWeatherCard></OtherWeatherCard>
  </div>
</template>

<script>
import { ref, provide } from "vue";
import WeatherCard from "./components/WeatherCard.vue";
import OtherWeatherCard from "./components/UI/OtherWeatherCard.vue";

export default {
  name: "App",
  components: {
    WeatherCard,
    OtherWeatherCard,
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
