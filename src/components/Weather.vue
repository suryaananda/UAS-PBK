<template>
    <div class="weather-widget">
      <q-input filled v-model="searchQuery" label="Masukkan Nama Kota di Indonesia" class="location-input" />
      <q-btn @click="searchWeather" class="search-button">Cari</q-btn>
      <div v-if="loading" class="loading-message">Loading data...</div>
      <div v-else-if="weatherData">
        <div class="weather-result">
          <div class="weather-info">
            <p class="city-name">{{ weatherData.location.name }}</p>
            <p class="temperature">{{ weatherData.current.temp_c }}°C</p>
            <img :src="weatherData.current.condition.icon" :alt="weatherData.current.condition.text" class="weather-icon" />
            <p class="weather-description">{{ weatherData.current.condition.text }}</p>
          </div>
          <div class="additional-info">
            <p>Kelembaban: {{ weatherData.current.humidity }}%</p>
            <p>Kecepatan Angin: {{ weatherData.current.wind_kph }} km/jam</p>
          </div>
        </div>
      </div>
      <div v-else-if="error" class="error-message">{{ error }}</div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue'
  import axios from 'axios'
  import { QInput, QBtn } from 'quasar'
  
  export default {
    components: { QInput, QBtn },
    setup() {
      const searchQuery = ref('')
      const weatherData = ref(null)
      const loading = ref(false)
      const error = ref(null)
  
      const searchWeather = async () => {
        loading.value = true
        error.value = null
  
        try {
          const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${searchQuery.value},ID&appid=bcbc6cc1860d02bd1f4306314c08d7e0&units=metric`)
          weatherData.value = response.data
        } catch (error) {
          console.error(error)
          error.value = 'Gagal mengambil data cuaca'
        } finally {
          loading.value = false
        }
      }
  
      return {
        searchQuery,
        weatherData,
        loading,
        error,
        searchWeather
      }
    }
  }
  </script>
  
  <style scoped>
  .weather-widget {
    max-width: 400px;
    margin: 0 auto;
  }
  
  .location-input {
    width: 100%;
    margin-bottom: 10px;
  }
  
  .search-button {
    width: 100%;
    padding: 10px 20px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .search-button:hover {
    background-color: #0056b3;
  }
  
  .loading-message {
    font-style: italic;
    color: #888;
    margin-top: 10px;
  }
  
  .weather-result {
    margin-top: 20px;
  }
  
  .weather-info {
    margin-bottom: 10px;
  }
  
  .city-name {
    font-weight: bold;
  }
  
  .temperature {
    font-size: 24px;
  }
  
  .weather-icon {
    width: 50px;
    height: 50px;
  }
  
  .weather-description {
    font-style: italic;
  }
  
  .additional-info {
    margin-top: 10px;
  }
  
  .additional-info p {
    margin: 5px 0;
  }
  
  .error-message {
    color: red;
    margin-top: 10px;
  }
  </style>
  