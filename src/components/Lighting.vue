<template>
  <div>
    <h2>Lighting</h2>
    <button :class="classname" v-if="lights" v-on:click="lightsOff()">
      POOL LIGHTS
    </button>
    <button :class="classname" v-else v-on:click="lightsOn()">
      POOL LIGHTS
    </button>
    {{ outlet }}
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
    lightsOn() {
      this.lights = true;
      this.classname = "on";
    },
    lightsOff() {
      this.lights = false;
      this.classname = "off";
    },
  },

  ///////////////////////////////////////////////////////////////////////////////
  //  MOUNTED
  ///////////////////////////////////////////////////////////////////////////////
  mounted() {
    this.classname = "off";
    this.lights = false;

    //let access_token = "Y2hpbmFzcHJvdXQ";
    let url = "http://192.168.1.100/restapi/relay/outlets/=0,1,4/state/";
    let base64 = "YWRtaW46MTIzNA==";
    let headers = new Headers();

    //headers.append('Content-Type', 'text/json');
    headers.append("Authorization", "Basic " + base64);

    fetch(url, { method: "GET", headers: headers })
      .then((response) => response.json())
      .then((outlet) => console.log(outlet));
    //.done();
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
