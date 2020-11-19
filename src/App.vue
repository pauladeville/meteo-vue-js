<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>

      <div class="search-box">
        <input
          type="text"
          placeholder="Tapez le nom d'une ville..."
          class="search-bar"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div v-if="typeof weather.main != 'undefined'" class="weather-wrap">

        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <img :src="iconUrl" alt="">
          <div class="temp">{{ Math.round(weather.main.temp)}}°C</div>
        </div>

      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: "65237321856bdb6b083f32e62c7ddccf",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      iconUrl: ""
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json()
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
      this.iconUrl = `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@2x.png`
    },
    dateBuilder () {
      let d = new Date();
      let months = ["janvier", "février", "mars", "avril", "mai", "juin", "juillet", "août", "septembre", "octobre", "novembre", "décembre"];
      let days = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Montserrat', sans-serif;
  max-width: 600px;
  margin: auto;

}
#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.5));
}
.search-box {
  width: 70%;
  margin: 50px auto;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px ;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  margin-top: 20px;
}
.weather-box {
  text-align: center;
}
.weather-box img {
  display: block;
  margin: 50px auto;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style:italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
