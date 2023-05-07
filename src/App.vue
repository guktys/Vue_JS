<template>
  <div :class="typeof weather.city != 'undefined' && weather.list[0].main.temp < 0 ? 'cold' : ''">
    <div class="main_box">
      <div class="app-wrap">
        <div class="search-box">

          <input type="text" class="search-input" v-model="query" @keypress="fetchWeather">
        </div>
        <i class="wi wi-day-sunny" v-if="weather.list && weather.list[0].weather[0].main === 'Clear'"></i>
        <i class="wi wi-cloudy" v-if="weather.list && weather.list[0].weather[0].main === 'Clouds'"></i>
        <i class="wi wi-rain" v-if="weather.list && weather.list[0].weather[0].main === 'Rain'"></i>
        <i class="wi wi-snow" v-if="weather.list && weather.list[0].weather[0].main === 'Snow'"></i>
        <div class="info-wrap" v-if="typeof weather.city != 'undefined'">

          <div class="info-box">

            <p class="date">{{ dateBuilder() }}</p>


            <div class="weather-box" >
              <h3 class="location">{{ weather.city.name }}, {{ weather.city.country }}</h3>
              <p class="temperature">{{ Math.round(weather.list[0].main.temp) }}°C</p>
              <p class="details">{{ weather.list[0].weather[0].main }}</p>

            </div>

          </div>


          <div class="forecast-box">
            <div v-for="(image, index) in forecastImages" :key="index" style="display: inline-block">
              <p>{{ forecastDays[index] }}</p>

              <p>{{ forecastWeather[index] }}°C</p>
              <p><img :src="'http://openweathermap.org/img/w/' + image + '.png'"/></p>
              <p class="detail">{{ forecastDetails[index] }}</p>
            
            </div>
          </div>

        </div>
      </div>

    </div>


  </div>


</template>

<script>
import moment from 'moment';

export default {
  name: 'App',
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}forecast?q=${this.query}&units=metric&APPID=${this.api_key}&cnt=5`).then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let date = moment().format('MMMM Do YYYY');
      return date;
    }
  },
  computed: {
    forecastDays() {
      let days = [];
      for (let i = 1; i <= 4; i++) {
        days.push(moment().add(i, 'days').format("ddd"));
      }
      return days;
    }, forecastWeather() {
      let temperature = [];
      for (let i = 1; i <= 4; i++) {
        temperature.push(Math.round(this.weather.list[i].main.temp));
      }
      return temperature;
    }, forecastImages() {
      let images = [];
      for (let i = 1; i <= 4; i++) {
        images.push(this.weather.list[i].weather[0].icon);
      }
      return images;
    }, forecastDetails() {
      let details = [];
      for (let i = 1; i <= 4; i++) {
        details.push(this.weather.list[i].weather[0].main);
      }
      return details;
    }

  },
  created() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        fetch(`${this.url_base}forecast?lat=${position.coords.latitude}&lon=${position.coords.longitude}&units=metric&APPID=${this.api_key}&cnt=5`).then(res => {
          return res.json();
        }).then(this.setResults);
      });
    } else {
      console.log("Your browser does not support geolocation API")
    }
  }
}

</script>
<style >

.main_box {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
