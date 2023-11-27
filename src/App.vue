<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 15 ? 'warm' :'cold' ">
    <main>
      <div class="searchBox">
        <input 
        type="text" 
        class="searchBar" 
        placeholder="Search Here!"
        v-model="query"
        @keypress="fetchWeather"
         />
      </div>
      <div class="weatherBox" v-if= "typeof weather.main != 'undefined'">
        <div class="locationBox">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{currentDate()}}</div>
        </div>
        <div class="weatherInfo">
        <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
        <div class="weather">{{weather.weather[0].main}}</div>
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
      api_key: 'fa43c1ad71717c5fe273cf59832ffb5e',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res =>{
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results){
      this.weather =results;
    },
    currentDate (){
      const date =new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let today = date.getDate();
      let day = days[date.getDay()];
      let month = months[date.getMonth()];
      let year =date.getFullYear();
      return `${day} ${today} ${month} ${year}`;
    }
  }
}
</script>

<style>
#app{
  background-image: url('./assets/default-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/hot-bg.jpg');
}
#app.cold{
  background-image: url('./assets/cold-bg.jpg');
}

* {
  margin: 0;
  padding: 0;
  box-sizing:border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0, 0, 0, 0.05));
}

.searchBox{
  width: 100%;
  margin-bottom: 30px;
}
.searchBox .searchBar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #181111;
  font-size: 20px;
  appearance: none;
  background: none;
  border: none;
  outline: none;
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 16px 16px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
}

.searchBox .searchBar:focus{
  background-color: rgba(255, 255, 255, 0.9);
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  border-radius: 50px 50px 50px 50px;
  color: #020e75 !important;
  outline: 2px solid #cffaf9;
}

.locationBox .location {
  color:white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.locationBox .date {
  color:white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weatherInfo{
  text-align: center;
}

.weatherInfo .temperature{
  display: inline-block;
  padding: 10px 25px;
  color:white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weatherInfo .weather{
  color:white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
