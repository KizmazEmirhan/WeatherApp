<template>
  <div
    class="z-[9999] bg-white flex flex-col rounded-lg p-4 gap-4 h-[fit-content] items-center"
  >
    <div id="city-informations">
      <div class="flex gap-2">
        <div class="flex gap-2 items-center text-4xl">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            fill="currentColor"
            class="bi bi-geo-alt"
            viewBox="0 0 16 16"
          >
            <path
              d="M12.166 8.94c-.524 1.062-1.234 2.12-1.96 3.07A32 32 0 0 1 8 14.58a32 32 0 0 1-2.206-2.57c-.726-.95-1.436-2.008-1.96-3.07C3.304 7.867 3 6.862 3 6a5 5 0 0 1 10 0c0 .862-.305 1.867-.834 2.94M8 16s6-5.686 6-10A6 6 0 0 0 2 6c0 4.314 6 10 6 10"
            />
            <path
              d="M8 8a2 2 0 1 1 0-4 2 2 0 0 1 0 4m0 1a3 3 0 1 0 0-6 3 3 0 0 0 0 6"
            />
          </svg>
          {{ weatherData?.location?.name }}
        </div>
      </div>
    </div>
    <div
      id="weather-content"
      class="flex gap-4 items-center text-3xl text-gray-400"
    >
      Sıcaklık: {{ weatherData?.current?.temp_c }}°C
      <img v-if="selectedIcon" :src="selectedIcon" alt="icon" />
    </div>
    <FutureWeather :latitude="lat" :longitude="long" :place="place"></FutureWeather>
  </div>
</template>

<script>
import axios from "axios";
import FutureWeather from "./UI/FutureWeather.vue";

const weatherConditions = [
  {
    code: 1000,
    day: "Sunny",
    night: "Clear",
    icon: 113,
  },
  {
    code: 1003,
    day: "Partly cloudy",
    night: "Partly cloudy",
    icon: 116,
  },
  {
    code: 1006,
    day: "Cloudy",
    night: "Cloudy",
    icon: 119,
  },
  {
    code: 1009,
    day: "Overcast",
    night: "Overcast",
    icon: 122,
  },
  {
    code: 1030,
    day: "Mist",
    night: "Mist",
    icon: 143,
  },
  {
    code: 1063,
    day: "Patchy rain possible",
    night: "Patchy rain possible",
    icon: 176,
  },
  {
    code: 1066,
    day: "Patchy snow possible",
    night: "Patchy snow possible",
    icon: 179,
  },
  {
    code: 1069,
    day: "Patchy sleet possible",
    night: "Patchy sleet possible",
    icon: 182,
  },
  {
    code: 1072,
    day: "Patchy freezing drizzle possible",
    night: "Patchy freezing drizzle possible",
    icon: 185,
  },
  {
    code: 1087,
    day: "Thundery outbreaks possible",
    night: "Thundery outbreaks possible",
    icon: 200,
  },
  {
    code: 1114,
    day: "Blowing snow",
    night: "Blowing snow",
    icon: 227,
  },
  {
    code: 1117,
    day: "Blizzard",
    night: "Blizzard",
    icon: 230,
  },
  {
    code: 1135,
    day: "Fog",
    night: "Fog",
    icon: 248,
  },
  {
    code: 1147,
    day: "Freezing fog",
    night: "Freezing fog",
    icon: 260,
  },
  {
    code: 1150,
    day: "Patchy light drizzle",
    night: "Patchy light drizzle",
    icon: 263,
  },
  {
    code: 1153,
    day: "Light drizzle",
    night: "Light drizzle",
    icon: 266,
  },
  {
    code: 1168,
    day: "Freezing drizzle",
    night: "Freezing drizzle",
    icon: 281,
  },
  {
    code: 1171,
    day: "Heavy freezing drizzle",
    night: "Heavy freezing drizzle",
    icon: 284,
  },
  {
    code: 1180,
    day: "Patchy light rain",
    night: "Patchy light rain",
    icon: 293,
  },
  {
    code: 1183,
    day: "Light rain",
    night: "Light rain",
    icon: 296,
  },
  {
    code: 1186,
    day: "Moderate rain at times",
    night: "Moderate rain at times",
    icon: 299,
  },
  {
    code: 1189,
    day: "Moderate rain",
    night: "Moderate rain",
    icon: 302,
  },
  {
    code: 1192,
    day: "Heavy rain at times",
    night: "Heavy rain at times",
    icon: 305,
  },
  {
    code: 1195,
    day: "Heavy rain",
    night: "Heavy rain",
    icon: 308,
  },
  {
    code: 1198,
    day: "Light freezing rain",
    night: "Light freezing rain",
    icon: 311,
  },
  {
    code: 1201,
    day: "Moderate or heavy freezing rain",
    night: "Moderate or heavy freezing rain",
    icon: 314,
  },
  {
    code: 1204,
    day: "Light sleet",
    night: "Light sleet",
    icon: 317,
  },
  {
    code: 1207,
    day: "Moderate or heavy sleet",
    night: "Moderate or heavy sleet",
    icon: 320,
  },
  {
    code: 1210,
    day: "Patchy light snow",
    night: "Patchy light snow",
    icon: 323,
  },
  {
    code: 1213,
    day: "Light snow",
    night: "Light snow",
    icon: 326,
  },
  {
    code: 1216,
    day: "Patchy moderate snow",
    night: "Patchy moderate snow",
    icon: 329,
  },
  {
    code: 1219,
    day: "Moderate snow",
    night: "Moderate snow",
    icon: 332,
  },
  {
    code: 1222,
    day: "Patchy heavy snow",
    night: "Patchy heavy snow",
    icon: 335,
  },
  {
    code: 1225,
    day: "Heavy snow",
    night: "Heavy snow",
    icon: 338,
  },
  {
    code: 1237,
    day: "Ice pellets",
    night: "Ice pellets",
    icon: 350,
  },
  {
    code: 1240,
    day: "Light rain shower",
    night: "Light rain shower",
    icon: 353,
  },
  {
    code: 1243,
    day: "Moderate or heavy rain shower",
    night: "Moderate or heavy rain shower",
    icon: 356,
  },
  {
    code: 1246,
    day: "Torrential rain shower",
    night: "Torrential rain shower",
    icon: 359,
  },
  {
    code: 1249,
    day: "Light sleet showers",
    night: "Light sleet showers",
    icon: 362,
  },
  {
    code: 1252,
    day: "Moderate or heavy sleet showers",
    night: "Moderate or heavy sleet showers",
    icon: 365,
  },
  {
    code: 1255,
    day: "Light snow showers",
    night: "Light snow showers",
    icon: 368,
  },
  {
    code: 1258,
    day: "Moderate or heavy snow showers",
    night: "Moderate or heavy snow showers",
    icon: 371,
  },
  {
    code: 1261,
    day: "Light showers of ice pellets",
    night: "Light showers of ice pellets",
    icon: 374,
  },
  {
    code: 1264,
    day: "Moderate or heavy showers of ice pellets",
    night: "Moderate or heavy showers of ice pellets",
    icon: 377,
  },
  {
    code: 1273,
    day: "Patchy light rain with thunder",
    night: "Patchy light rain with thunder",
    icon: 386,
  },
  {
    code: 1276,
    day: "Moderate or heavy rain with thunder",
    night: "Moderate or heavy rain with thunder",
    icon: 389,
  },
  {
    code: 1279,
    day: "Patchy light snow with thunder",
    night: "Patchy light snow with thunder",
    icon: 392,
  },
  {
    code: 1282,
    day: "Moderate or heavy snow with thunder",
    night: "Moderate or heavy snow with thunder",
    icon: 395,
  },
];
export default {
  components: {
    FutureWeather,
  },

  props: {
    place: {
      type: String,
      default: null,
    },
    lat: { type: Number, default: null },
    long: { type: Number, default: null },
  },
  mounted() {
    this.getWeatherData(this.latitude, this.longtitude);
    // this.$watch(
    //   () => [this.latitude, this.longtitude],
    //   ([newLat, newLong]) => {
    //     this.getWeatherData(newLat, newLong);
    //   }
    // );
  },
  data() {
    return {
      API_KEY: "3ed874c8754016794d6a163d755b1f42",
      weatherData: null,
      weatherDesc: "",
      selectedIcon: null,
    };
  },
  watch: {
    lat: {
      immediate: true,
      handler(newLat) {
        if (newLat) {
          this.getWeatherData(newLat, this.long);
        }
      },
    },
    place: {
      immediate: true,
      handler(newPlace) {
        if (newPlace) {
          this.getWeatherData(newPlace);
        }
      },
    },
  },
  methods: {
    setWeatherInfo(code) {
      const condition = weatherConditions.find((cond) => cond.code === code);
      if (condition) {
        const isDay = this.weatherData.current.is_day;
        this.weatherDesc = isDay ? condition.day : condition.night;
        this.selectedIcon = `https://cdn.weatherapi.com/weather/64x64/day/${condition.icon}.png`;
      } else {
        this.weatherDesc = "Bilinmeyen Hava Durumu";
        this.selectedIcon = null;
      }
    },
    getWeatherData(lat, long, place) {
      console.log("fonksiyondaki place =", place);
      const query = place ? place : `${lat},${long}`;
      console.log(query);
      axios
        .get(`https://api.weatherapi.com/v1/current.json`, {
          params: {
            key: "d72c5a037a5745808e6141937240609",
            q: query,
          },
        })
        .then((response) => {
          this.weatherData = response.data;
          const weatherCode = response.data.current.condition.code;
          this.setWeatherInfo(weatherCode);
        })
        .catch((error) => {
          console.error("API request failed:", error);
        });
    },
  },
};
</script>
