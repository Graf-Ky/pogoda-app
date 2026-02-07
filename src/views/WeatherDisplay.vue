<template>



  {{ getWeatherIcon(weather.current_weather.weathercode) }}  
  <p>Погода в городе {{ city }} : 🌡 {{ weather.current_weather.temperature }}°C</p>
  <p>💨 Ветер : {{ weather.current_weather.windspeed }} км/ч </p>>
  
  <button @click="close">скрыть</button>


</template>
 



<script setup>
defineProps({
  weather: Object,
  city: String
})


function getWeatherIcon(code){
  if (code === 0) return '☀️'
  if (code === 1) return '🌤️'
  if (code === 2) return  '⛅'  
  if (code === 3) return  '⛅'   
  if (code === 45) return  "🌫️" 
  if (code === 48) return  "🌫️" 
  if (code === 71) return  "❄️🌨️" 
  if (code === 75) return  "❄️🌨️"
}


const emit = defineEmits(["close"])
function close(){
  emit("close")
}
</script>