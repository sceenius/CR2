<template>
  <div>
    <h2>Lighting</h2>
    <button class="off" v-if="outlet[0]" v-on:click="lightsOff()">
      LIGHT 1
    </button>
    <button class="on" v-else v-on:click="lightsOn()">LIGHT 1</button>
    {{ outlet[0] }}
  </div>
</template>

<script>
export default {
  name: "Lighting",
  getHoursCondition: String,
  lights: false,
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
      outlet: {},
    };
  },

  ///////////////////////////////////////////////////////////////////////////////
  //  METHODS
  ///////////////////////////////////////////////////////////////////////////////
  methods: {
    lightsOn(outlets) {
      this.lights = true;
      this.classname = "on";
      let url = `https://192.168.1.100/restapi/relay/outlets/${outlet}/state/`;
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
        .then((response) => response.json())
        .then((outlet) => console.log(outlet))
        .catch((error) => console.log(error));
    },
    lightsOff(outlet) {
      this.lights = false;
      this.classname = "off";
      let url = `https://192.168.1.100/restapi/relay/outlets/${outlet}/state/`;
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
        .then((response) => response.json())
        .then((outlet) => console.log(outlet))
        .catch((error) => console.log(error));
    },
  },

  ///////////////////////////////////////////////////////////////////////////////
  //  MOUNTED
  ///////////////////////////////////////////////////////////////////////////////
  mounted() {
    let url = "https://192.168.1.100/restapi/relay/outlets/all;/state/";
    let base64 = "YWRtaW46MTIzNA==";
    let headers = new Headers();

    headers.append("Content-Type", "application/json");
    headers.append("Authorization", "Basic " + base64);

    fetch(url, { method: "GET", headers: headers })
      .then((response) => response.json())
      .then((outlet) => console.log(outlet))
      .catch((error) => console.log(error));
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
  width: 300px;
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
  box-shadow: 0 5px 15px rgba(255, 255, 255, 1);
}
.off {
  opacity: 0.9;
}
</style>
