<template>
  <div>
    <h2>Lighting</h2>
    <button class="on" ref="B1" v-if="outlets[0]" v-on:click="lightsOff(0)">
      LIGHT 1
    </button>
    <button class="off" ref="B1" v-else v-on:click="lightsOn(0)">
      LIGHT 1
    </button>

    <button class="on" ref="B2" v-if="outlets[1]" v-on:click="lightsOff(1)">
      LIGHT 2
    </button>
    <button class="off" ref="B2" v-else v-on:click="lightsOn(1)">
      LIGHT 2
    </button>

    <button class="on" ref="B3" v-if="outlets[2]" v-on:click="lightsOff(2)">
      LIGHT 3
    </button>
    <button class="off" ref="B3" v-else v-on:click="lightsOn(2)">
      LIGHT 3
    </button>
  </div>
</template>

<script>
export default {
  name: "Lighting",
  getHoursCondition: String,
  classname: "on",
  props: {},

  ///////////////////////////////////////////////////////////////////////////////
  //  DATA
  ///////////////////////////////////////////////////////////////////////////////
  data() {
    return {
      hours: new Date().getHours(),
      getHoursCondition: "", //define the variable first
      lights: false,
      classname: "on",
      outlets: {},
      text: "LIGHT 1",
    };
  },

  ///////////////////////////////////////////////////////////////////////////////
  //  METHODS
  ///////////////////////////////////////////////////////////////////////////////
  methods: {
    lightsOn(outlet) {
      this.$refs[`B${outlet + 1}`].innerText = "...";
      this.$forceUpdate();
      let url = `https://201.182.226.142:5000/restapi/relay/outlets/${outlet}/state/`;
      let base64 = "YWRtaW46MTIzNA==";
      let headers = new Headers();

      headers.append("Content-Type", "application/json");
      headers.append("Authorization", "Basic " + base64);
      //headers.append("Accept", "application/json");
      //headers.append("X-Requested-With", "XMLHttpRequest");
      //headers.append("X-CSRF", "x");

      fetch(url, {
        method: "PUT",
        headers: headers,
        body: JSON.stringify(true),
      })
        .then((response) => response.text())
        .then(() => (this.outlets[outlet] = true))
        .then(() => this.$forceUpdate())
        .then(
          () => (this.$refs[`B${outlet + 1}`].innerText = `LIGHT ${outlet + 1}`)
        )
        .catch((error) => console.log(error));
    },

    lightsOff(outlet) {
      this.$refs[`B${outlet + 1}`].innerText = "...";
      let url = `https://201.182.226.142:5000/restapi/relay/outlets/${outlet}/state/`;
      let base64 = "YWRtaW46MTIzNA==";
      let headers = new Headers();

      headers.append("Content-Type", "application/json");
      headers.append("Authorization", "Basic " + base64);
      //headers.append("Accept", "application/json");
      //headers.append("X-Requested-With", "XMLHttpRequest");
      //headers.append("X-CSRF", "x");

      fetch(url, {
        method: "PUT",
        headers: headers,
        body: JSON.stringify(false),
      })
        .then((response) => response.text())
        .then(() => (this.outlets[outlet] = false))
        .then(() => this.$forceUpdate())
        .then(
          () => (this.$refs[`B${outlet + 1}`].innerText = `LIGHT ${outlet + 1}`)
        )
        .catch((error) => console.log(error));
    },

    lightsAll: function () {
      // Both requesting site and API call must be secure
      let url =
        "https://201.182.226.142:5000/restapi/relay/outlets/all;/state/";
      let base64 = "YWRtaW46MTIzNA==";
      let headers = new Headers();

      headers.append("Content-Type", "application/json");
      headers.append("Authorization", "Basic " + base64);

      fetch(url, { method: "GET", headers: headers })
        .then((response) => response.json())
        .then((outlets) => (this.outlets = outlets))
        .then((outlets) => console.log(outlets))
        .catch((error) => console.log(error));
    },
  },

  ///////////////////////////////////////////////////////////////////////////////
  //  MOUNTED
  ///////////////////////////////////////////////////////////////////////////////
  mounted() {
    this.lightsAll();
  },
};
</script>

<style>
h3 {
  margin: -140px 0 100 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

button {
  background-color: #F1C232;
  width: 200px;
  border: none;
  color: #fff;
  padding: 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 26px;
  margin: 54px 2px;
  border-radius: 35px;
  cursor: pointer;
}

.on {
  opacity: 1;
  border: none;
  /* box-shadow: 0 5px 15px rgba(255, 255, 255, 1); */
}
.off {
  background-color: transparent;
  border: 2px solid white;
  opacity: 0.9;
}
</style>
