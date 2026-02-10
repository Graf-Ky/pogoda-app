<template>

<p>(возможны иные прогнозы из-за схожести названий городов)</p>
<p1>рекомендация: не Москва, а Moscow - надежнее написать запрос в некоторых случаях </p1>
<hr> </hr>
<div style="padding: 20px; max-width: 500px; margin: 0 auto;">
<CitySearch @search="handleCitySearch" />


<WeatherDisplay
v-if="weather"
:weather="weather"
:city="city"
  @close="weather = null" 
/>
</div>  




</template>
 



<script setup>
import { ref, onMounted, onUnmounted } from "vue"
import WeatherDisplay from "./WeatherDisplay.vue"
import CitySearch from "./CitySearch.vue"

const weather = ref(null)
const city =  ref("")





async function searchWeather() {
  weather.value = null
  try{
  const response = await fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${city.value}`)
  const data =await response.json()
                                              //city.value = data
  console.log(data)
  if(data.results && data.results.length > 0){
  const firstCity = data.results[0]
  console.log("Город:", firstCity.name)
  console.log("Широта:", firstCity.latitude)
  console.log("Долгота:", firstCity.longitude)

  const  weatherData = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${firstCity.latitude}&longitude=${firstCity.longitude}&current_weather=true`)
  const data2 = await weatherData.json()
  weather.value = data2 
  console.log(data2)
 
  }
  }catch(error){
    console.log("OSHIBKA", error)
  }
}


let timer = setInterval(()=>{
  if(city.value){
    searchWeather()
}
}, 600000)

onUnmounted(() => {
  clearInterval(timer)
})


function handleCitySearch(cityName){
  city.value =cityName
  searchWeather()
}
</script>
