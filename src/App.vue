<template>
  <div>
    <h1>Caribbean Courtyard Villa</h1>
    <Lighting />
    <Weather />
  </div>
</template>

<script>
import LightingVue from "./components/Lighting.vue";
import WeatherVue from "./components/Weather.vue";

export default {
  name: "App",
  components: {
    Lighting: LightingVue,
    Weather: WeatherVue,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Rubik");
h1 {
  font-size: 3em;
  color: #fff;
}
</style>
