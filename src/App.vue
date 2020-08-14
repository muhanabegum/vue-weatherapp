<template>
  <div id="app">
    <main>
      <div class='search-box'>
        <input type="text" class="search-bar" placeholder="Type in a city..." v-model="query" @keypress="fetchWeather"/>
      </div>

      <div class='weather-wrap' v-if="typeof weather.main != 'undefined'">
        <div class='location-box'>
          <div class='location'>
            {{weather.main}}, {{weather.sys.country}}
          </div>
          <div class='date'>
            {{ dateBuilder() }}
          </div>
        </div>
        <div class='weather-box'>
          <div class='temp'>{{ Math.round(weather.main.temp) }}°c</div>
          <div class='precip'>{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: ['please insert your api key here'],
      url_base: 'http://api.openweathermap.org/data/2.5',
      query: '',
      weather:{}

    }
  },
  methods: {
    fetchweather(e) {
      if (e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res=>{
            return res.json(); 
          }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather=results; 
    },
     dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}

</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Roboto'); 
body{
  font-family: 'Roboto'; 
}
  #app{
  background-image: url(assets/background.jpg); 
  background-size: cover; 
  background-position: bottom; 
  transition: 0.4s; 
  }

main{
  min-height: 100vh; 
  padding: 25px; 
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75)); 

}

.search-box{
  width: 98%; 
  margin-bottom: 120px; 
  margin-top: 20px;
}

.search-box .search-bar{
  display:block; 
  width: 100%; 
  padding: 15px; 
  color: black; 
  font-size: 20px; 
  appearance: none; 
  border: none; 
  outline: none; 
  background: none; 
  border-radius: 0px 16px 0px 16px; 
  transition: 0.4s; 
}

.search-box .search-bar:focus{
  border-radius: 16px 0px 16px 0px; 
}

.location-box .location{
  color: white; 
  font-size:32px; 
  font-weight: 500; 
  text-align: center; 
}

.location-box .date{
  color: white; 
  font-size:20px; 
  font-weight: 300; 
  text-align: center; 
  font-style: italic;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block; 
  padding: 10px 25px; 
  color: white; 
  font-size: 102px; 
  font-weight: 900px; 
  border-radius: 16px; 
  margin: 30px 0px; 
}

.weather-box .precip{
  color: white;
  font-size: 48px; 
  font-weight: 700; 
  font-style: italic; 
}
</style>

