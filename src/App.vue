<template>
  <div id="app">
    <div class="nav">
      <div class="logo">Weathering With {{main}}</div>
      <div style="margin-top: 20px; display: flex; align-items: center;">
        <input type="text" placeholder="지역이름을 영어로" autofocuss />
        <button type="button" @click="searchWeather">검색</button>
      </div>
    </div>
    <div v-if="this.view === true" class="main">
      <h2>국가명 : {{ country }}</h2>
      <p>도시명 : {{ city }}</p>
      <p>현재온도 : {{nowtemps}}도</p>
      <p>오늘 최저온도 : {{mintemp}}도</p>
      <p>오늘 최고온도 : {{maxtemp}}도</p>
      <p>날씨 : {{sky}}</p>
      <p>덧붙여 : {{des}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      view: false,
      main: "",
      country: "",
      city: "",
      nowtemps: "",
      mintemp: "",
      maxtemp: "",
      sky: "",
      des: ""
    };
  },
  computed: {
    hasResult: function() {
      return this.posts.length > 0;
    }
  },
  methods: {
    searchWeather() {
      var x = document.getElementsByTagName("input")[0].value.toUpperCase();
      const BASE_URL =
        "http://api.openweathermap.org/data/2.5/weather?q=" +
        x +
        "&appid=1cfd06402c98f3957adcf45fb03bb5b4";
      this.$http
        .get(`${BASE_URL}`)
        .then(result => {
          var m = result.data;
          this.country = m.sys.country;
          this.city = m.name;
          this.nowtemps = String(result.data.main.temp - 272).substring(0, 4);
          this.mintemp = String(result.data.main.temp_min - 272).substring(
            0,
            4
          );
          this.maxtemp = String(result.data.main.temp_max - 272).substring(
            0,
            4
          );
          this.sky = m.weather[0].main;
          this.des = m.weather[0].description;
          this.view = true;
          if (this.view == true) {
            this.main = x;
          }
        })
        .catch(err => {
          alert("다시 입력해주세요");
        });
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background: #36afff;
  background-image: url("./assets/1.jpg");
  height: 100vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

#app {
  font-family: "Ubuntu", sans-serif;
  text-align: center;
  display: flex;
  flex-flow: column;
  text-shadow: -0.2px 0 black, 0 0.2px black, 0.2px 0 black, 0 -0.2px black;
  color: white;
}
.nav {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column;
  margin-top: 8vw;
}
.logo {
  font-size: 7vw;
}
input {
  width: 20vw;
  height: 3vh;
  padding-left: 30px;
  border: none;
  opacity: 0.7;
  border-radius: 3px;
}
button {
  width: 3.3vw;
  height: 3.3vh;
  border-radius: 3px;
  margin-left: 10px;
  font-size: 0.8vw;
  border: none;
  transition-duration: 444ms;
}
@media (max-width: 640px) {
  button {
    width: 8vw;
    height: 3vh;
    border-radius: 3px;
    margin-left: 10px;
    border: none;
    transition-duration: 444ms;
  }
  input {
    width: 20vw;
    height: 3vh;
    padding-right: 10px;
    border: none;
    opacity: 0.7;
    border-radius: 3px;
    font-size: 5pt;
  }
}
button:hover {
  background: #2c3e50;
  color: white;
  text-shadow: none;
}
.main {
  width: 80vw;
  margin: 4vw 10vw;
  background: white;
  color: #36afff;
  opacity: 0.8;
  transition-duration: 444ms linear;
}
p,
h2 {
  transition: all 555ms ease;
  text-shadow: none;
}
h2:hover {
  font-size: 1.7em;
}
p:hover {
  font-size: 1.2em;
}
h2:hover,
p:hover {
  text-shadow: 0 0 0.3em #4db8ff;
  cursor: default;
}
</style>
