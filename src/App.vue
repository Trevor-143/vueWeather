<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <div class="start">
      <main>
        <div class="search_box">
          <input type="text" class="search_bar" placeholder="search..." v-model="query" @keypress="fetchWeather">
        </div>

        <div class="weather_wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location_box">
            <div class="location">{{ weather.name }},  {{ weather.sys.country }}</div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>

          <div class="weather_box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>

        </div>
      </main>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  
  data() {
    return {
      api_key: '3cb2524118612e897d2126f3b973ba83',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key === 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key }`)
        .then(response => {
          return response.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results
    },
    dateBuilder() {
      let d = new Date();
      let months = ['January','February','March','April','May','June','July','August','September','October','November','December'];
      let days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    }
  }

}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap');
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'poppins', sans-serif;
  }
  #app {
    background-image: url('./assets/cold.jpg');
    background-size: cover;
    
    background-position: bottom;
    transition: 0.4s;
  }
  #app.warm {
    background-image: url('./assets/warm.jpeg');
  }
  .start {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
    display: flex;
    justify-content: center;
    align-items: center;
  }
  main {
    max-width: 600px;
  }
  .search_box {
    width: 100%;
    margin-bottom: 30px;
  }
  .search_box .search_bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 99px;
    transition: 0.4s;
  }
  .search_box .search_bar:focus {
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 99px;
  }
  .weather_wrap {
    text-align: center;
  }
  .location_box .location {
    color: #fff;
    font-size: 22px;
    font-weight: 500;
  }
  .location_box .date {
    color: #fff;
    font-size: 18px;
    font-weight: 300px;
  }
  .weather_box {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .weather_box .temp {
    display: inline-block;
    padding: 20px;
    color: #fff;
    width: 150px;
    height: 150px;
    font-size: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 600;
    background-color: rgba(255, 255, 255, 0.3);
    border-radius: 50%;
    margin: 30px;
  }
  .weather_box .weather {
    color: #fff;
    font-size: 20px;
    font-weight: 500;
    
  }

</style>
