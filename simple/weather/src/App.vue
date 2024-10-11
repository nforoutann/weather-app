<template>
  <div class="card">
    <div class="search">
      <input type="text" placeholder="enter the city" v-model="city">
      <button @click="checkWeather(city)">
        <img src="./assets/images/search.png" alt="search">
      </button>
    </div>

    <div v-if="!start" class="weather">

      <div v-if="!error">
        <img :src="imageUrl" class="weather-icon">
        <h1 class="temp"> {{temp}} °C</h1>
        <h2 class="city"> {{currentCity}} </h2>
        <div class="details">
          <div class="row">
            <img src="./assets/images/humidity.png">
            <div class="col">
              <div>{{humidity}} %</div>
              <div>Humidity</div>
            </div>
          </div>
          <div class="row">
            <img src="./assets/images/wind.png">
            <div class="col">
              <div>{{wind}} km/h</div>
              <div>Wind Speed</div>
            </div> 
          </div>
        </div>
      </div>
      <div v-else class="error">
        The city you are searching for is not existed!
      </div>

    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup(){
    const start = ref(true)
    const error = ref(false)
    const apiKey = ref('33500fc540660d5bcbdd4f22d783f862')
    const apiUrl = ref('https://api.openweathermap.org/data/2.5/weather?q=') 
    const city = ref('')
    const temp = ref(null)
    const humidity = ref(null)
    const wind = ref(null)
    const currentCity = ref(null)
    const imageUrl = ref('')
    


    async function checkWeather(city){
      const response = await fetch(`${apiUrl.value}${city}&appid=${apiKey.value}`)
      const data = await response.json()

      if(response.status !== 200){
        error.value = true
      } else{
        temp.value = Math.round(data.main.temp-273.15)
        humidity.value = data.main.humidity
        wind.value = data.wind.speed
        currentCity.value = data.name
        switch(data.weather[0].main){
          case "Clear": imageUrl.value = new URL('@/assets/images/clear.png', import.meta.url).href; break;
          case "Rain": imageUrl.value = new URL('@/assets/images/rain.png', import.meta.url).href; break;
          case "Clouds": imageUrl.value = new URL('@/assets/images/clouds.png', import.meta.url).href; break;
          case "Drizzle": imageUrl.value = new URL('@/assets/images/drizzle.png', import.meta.url).href; break;
          case "Mist":  imageUrl.value = new URL('@/assets/images/mist.png', import.meta.url).href; break;
        }

        error.value = false
      }
      start.value = false
    }


    return {start, error, city, checkWeather, temp, humidity, wind, currentCity, imageUrl}
  }
}
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: rgba(23, 23, 28, 0.94);
  padding-top: 40px;
}

.card{
  background: linear-gradient(135deg, #00feba, #5b548a);
  height: auto;
  width: 90%;
  max-width: 470px;
  margin: 100px auto 0;
  border-radius: 10px;
  padding: 40px;
  text-align: center;
}

.search{
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.search input{
  border: 0;
  outline: 0;
  background: #ebfffc;
  color: #555;
  padding: 10px 25px;
  height: 60px;
  border-radius: 30px;
  flex: 1;
  margin-right: 16px;
  font-size: 18px;
}

.search button{
  border: 0;
  outline: 0;
  background: #ebfffc;
  border-radius: 50%;
  width: 60px;
  height: 60px;
  cursor: pointer;
}

.search button img{
  width: 16px;
}

.details {
  display: flex;
  justify-content: space-between;
  padding: 20px;
}

.row{
  display: flex;
  align-items: center;
} 

.temp{
  color: white;
  font-size: 100px;
  margin: 0px;
  padding: 0;
}

.city{
  color: white;
  font-size: 40px;
  margin-bottom: 20px;
  margin-top: 0;
}

.col{
  margin-left: 10px;
  color: white;
}

.error{
  color: white;
  text-align: start;
  margin-top: 20px;
  margin-left: 5px;
}

</style>
