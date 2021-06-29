<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '' ">
    <main>
      <div class="search-box">
        <input type="text" placeholder="Search..." class="search-bar"
               v-model="query"
              @keypress="showWeather"
        >
      </div>

      <div class="weather-wap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">{{ weather.name }} {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

  export default {
    name: 'app',
    data() {
      return {
        api_key: 'db4f8758d0160e56733ac130461715db',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {}
      }
    },
    methods: {
      showWeather(e) {
        console.log(e.key)
        // e.preventDefault()
        if (e.key == 'Enter') {
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)

          .then(res => {
            return res.json()
          }).then(this.setResults)
        }
      },

      setResults(results) {
        this.weather = results;
      },

      dateBuilder() {
        let d = new Date()
        let months = ['Jaunary', 'Febrary', 'March', 'Aprel', 'May', 'June', 'July', 'Auguest', 'September','Oktabr', 'November','Desember'];
        let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

        let day = days[d.getDay()]
        let date = d.getDate()
        let month = months[d.getMonth()]
        let year = d.getFullYear()

        return `${day} ${date} ${month} ${year}`
      }
    }
  }

</script>

<style>

  *{
    padding: 0px;
    margin: 0px;
    box-sizing: border-box;
  }

  body {
    font-family: 'monserat', sans-serif;
  }
  #app{
    background-image: url("./assets/could.jpg");
    background-position: center;
    background-size: cover;
    transition: .4s;
  }
  #app.warm {
    background-image: url("./assets/warm.jpg") !important;

  }

  main {
    display: grid;
    grid-template-columns: 50% 50%;
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, .1), rgba(0, 0, 0, 2));
  }
  .search-box {
    width: 90%;
  }
  .search-box .search-bar {
    margin-top: 30vh;
    display: block;
    width: 100%;
    padding: 15px;
    color: #3d3434;
    font-size: 20px;
    border: none;
    background: none;
    outline: none;
    appearance: none;
    background-color: rgba(255, 255, 255, .5);
    border-radius: 0 16px 0 16px;
  }
  .search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, .25);
    background-color: rgba(255, 255, 255, .75);
    border-radius: 16px 0 16px 0;
  }
  .location-box .location {
    color: #ffffff;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, .25);
  }
  .location-box .date {
    color: #ffffff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }
  .weather-box {
    margin-top: 10vh;
    text-align: center;
  }
  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, .25);
    border-radius: 16px;
    margin: 30px 0;
    box-shadow: 3px 6px rgba(0, 0, 0, .25);
    background: rgba(255, 255, 255, .25);
  }
  .weather-box .weather {
    color: #ffffff;
    font-size: 28px;
    font-weight: 700;
    font-style: initial;
    text-shadow: 3px 6px rgba(0, 0, 0, .25);
  }

</style>
