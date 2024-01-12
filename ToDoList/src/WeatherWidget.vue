
<template>
  <div class="weather-widget">
    <div v-if="weatherData" class="weather-details" @mouseover="showMore = true" @mouseleave="showMore = false" >
      <p>Boston</p>
      <p class="temperature">{{ weatherData.temperature }}°C</p>
      <transition name="fade">
      <div v-show="showMore" class="additional-data">
        <p class="hovrtemperature">Humidity: {{ weatherData.humidity }}%</p>
        <p class="hovrtemperature">CloudCover: {{ weatherData.cloudCover }}%</p>
      </div>
      </transition>
    </div>
  </div>
</template>

<script>


export default {
  data() {
    return {
      weatherData: null,
      showMore: false
    };
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    async fetchWeatherData() {
      try {
        // const apiKey = '767ca9c391439e1addd64f3fcbbf1033';
        const apiKey ='1bigACY7MmRk9MTxVFmPVf9iNA5fLlcF'; //tomorrow.io
        const response = await fetch(
          // 'https://api.openweathermap.org/data/2.5/weather?lat=42.3601&lon=71.0589&appid=' + apiKey

          'https://api.tomorrow.io/v4/weather/realtime?location=boston&apikey='+ apiKey
        );

        const data = await response.json();
        console.log(data)

        // Assuming the API response structure, modify accordingly
        this.weatherData = {
          // temperature: data.main.temp,
          // weather: data.weather,
          temperature: data.data.values.temperature,
          humidity: data.data.values.humidity,
          cloudCover: data.data.values.cloudCover,
        };
      } catch (error) {
        console.error('Error fetching weather data:', error);
      }
    },
  },
};
</script>

<style scoped>
.weather-widget {
  max-width: 300px;
  width: 100%;
  margin: auto;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  background-color: #fff;
  overflow: hidden;
}

.weather-widget:hover {
  transform: scale(1.05);
}

.weather-details {
  text-align: center;
  background-color: #3498db;
  color: #fff;
  padding: 20px;
  border-radius: 15px;
  position: relative;
  transition: background-color 0.3s ease;
}

.weather-details:hover {
  background-color: #2c3e50;
}

.city {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

.temperature {
  font-size: 2rem;
  font-weight: bold;
}
.hovrtemperature {
  font-size: 1rem;
  font-weight: bold;
}

.additional-data {
  font-size: 1rem;
  padding-top: 10px;
  color: #fff;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
</style>