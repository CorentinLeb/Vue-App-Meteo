<template>
  <div id="app">
    <main>
      <div class="search-box">
        <form class="search-location"
        @submit.prevent="getWeather">
          <div class="search-box">
            <input 
            type="text"
            class="search-bar"
            placeholder="Entrez le nom de la ville" 
            v-model="citySearch" 
            autocomplete="off"/>
          </div>
        </form>
      </div>

      <div class="location-box">
          <h1 class="location">{{weather.cityName}}</h1>
        <div class="weather-box">
          <h2 class="temp">{{weather.temperature}}°C</h2>
          <p class="weather">{{weather.description}}</p>
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
        isDay: true,
        searchResult: false,
        cityDisplay: false,
        citySearch: "",
        weather: {
        cityName : "Bordeaux",
        temperature : 9,
        description : "Cloudy with a chance of meatballs",
      },
    }
  },  
    methods: {
    getWeather: async function() {
      console.log(this.citySearch);
      const key = "ec80fe5c759a7923eb1c40f97cf4d4bf";
      const callURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
      //? APPEL A l'API avec un await dans un try 
      try {
        const response = await fetch(callURL);
        const data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.searchResult = true;
        //check if it's daytime or nighttime or
        const dayNight = data.weather[0].icon;
        if(dayNight.includes("d")){
          this.isDay = true;
        }
        else{
          this.isDay = false;
        }
      } catch (error) {
        console.log(error)
      }
    }
  }
}

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-image: url('../src/assets/cold-bg.webp');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
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
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0, 0.25);
}

.location-box .date{
  color: white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  color: white;
  display: inline-block;
  padding: 10px 25px;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>