<template>
  <div id="principal" style="background: rgb(238 239 246)">
    <div class="sidebar">
      <img src="@/assets/leanTech.svg?data" style="margin-top: 30px; margin-bottom: 50px" />
      <img src="@/assets/dashboard.svg?data" />
    </div>
    <div class="container my-5" style="max-width: 500px; min-width: 400px; margin-left: 60px; font-size: 24px">
      <h1 class="title" style="color: #ff4e00; margin-bottom: 25px">Your Cities</h1>
      <form class="search-location" v-on:submit.prevent="getWeather">
        <input
          type="text"
          class="form-control text-muted form-rounded p-4 shadow-sm"
          style="border-radius: 0.75rem"
          placeholder="City"
          v-model="citySearch"
          autocomplete="off"
        />
      </form>
      <p class="text-center my-3" v-if="cityFound"><img src="@/assets/noWeather.svg?data" /></p>
      <div
        class="card rounded my-3 shadow-lg back-card overflow-hidden"
        style="max-height: 600px; min-height: 300px"
        v-if="visible"
      >
        <!-- Top of card starts here -->
        <div class="card-top text-center" style="margin-bottom: 2rem">
          <p style="color: rgb(8 0 78)">{{ weather.cityName }}, {{ weather.country }}</p>
        </div>
        <!-- top of card ends here -->

        <!--card middle body, card body used cos I want to just update the innerHTML -->
        <div class="card-body">
          <!-- card middle starts here -->
          <div class="card-mid">
            <div class="row"> 
              <div class="col-12 text-center temp">
                <span style="color: #ff4e00">{{ weather.temperature }}&deg;C</span>
                <p class="weather" v-if="weather.temperature <= 19"> <img src="@/assets/coldWeather.svg?data"/>Cold</p>
                <p class="weather" v-else-if="(weather.temperature > 19) && (weather.temperature <=26)"> <img src="@/assets/warmWeather.svg?data"/> Warm</p>
                <p class="weather" v-else><img src="@/assets/hotWeather.svg?data"/>  Hot</p>
              </div>
            </div>
          </div>
          <!-- card middle ends here -->

          <!-- card bottom starts here -->
          <div class="card-bottom px-5 py-4 row">
            
          </div>

          <!-- card bottom ends here -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {

  data() {
    return {
      cityFound: false,
      visible: false,
      stormy: false,
      cloudy: false,
      clearSky: false,
      clearNight: false,
      snowy: false,

      isDay: true,
      citySearch: "",
      weather: {
        cityName: "Gwarinpa",
        country: "NG",
        temperature: 12,
        description: "Clouds everywhere",
        lowTemp: "19",
        highTemp: "30",
        feelsLike: "20",
        humidity: "55",
      },
    };
  },
  methods: {
    getWeather: async function () {
      console.log(this.citySearch);
      const key = "a700ad143b075a9a32a744bea064feb3";
      const baseURL = `https://cors-anywhere.herokuapp.com/http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;

      //fetch weather
      try {
        const response = await fetch(baseURL);
        const data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);

        this.visible = true;
        this.cityFound = false;
        this.getIcon;

      } catch (error) {
        console.log(error);
        this.cityFound = true;
        this.visible = false;
      }
    }
  }
};
</script>

<style scoped>
@import "./assets/custom.css";
</style>
