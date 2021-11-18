<template>
  <div>
    <main>
      <h2>Weather</h2>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "Weather",

  getHoursCondition: String,
  lights: false,
  classname: "on",

  props: {},

  ///////////////////////////////////////////////////////////////////////////////
  //  DATA
  ///////////////////////////////////////////////////////////////////////////////
  data() {
    return {
      api_key: "e086d67ffe83c057839c3dadf9f24070",
      weather: {},
    };
  },

  ///////////////////////////////////////////////////////////////////////////////
  //  METHODS
  ///////////////////////////////////////////////////////////////////////////////
  methods: {
    async getWeather(location) {
      try {
        let response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${location}&units=metric&APPID=${this.api_key}`
        );
        this.weather = await response.json();
        if (this.weather.weather[0].icon.includes("n")) {
          document.body.className = "night";
        } else {
          document.body.className = "day";
        }
        console.log(this.weather);
      } catch (error) {
        console.log(error);
      }
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },

  ///////////////////////////////////////////////////////////////////////////////
  //  MOUNTED
  ///////////////////////////////////////////////////////////////////////////////
  mounted() {
    this.getWeather("Playa Chiquita, CR");
  },
};
</script>

<style>
body {
  font-family: "Rubik", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ddd;
  margin-top: 160px;
}
body.night {
  background: url("https://assets.hypha.earth/images/bg_night.png") no-repeat
    center center fixed;
  background-size: cover;
  background-color: #111;
}

body.day {
  background: url("https://assets.hypha.earth/images/bg_day.png") no-repeat
    center center fixed;
  background-size: cover;
  background-color: #ddd;
}
</style>
