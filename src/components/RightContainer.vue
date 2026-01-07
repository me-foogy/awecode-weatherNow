<script setup lang="ts">
    import DailyForecast from './DailyForecast.vue';
    import {ref, watch} from 'vue';
    const apiKey = import.meta.env.VITE_GEOCODING_API;
   
    interface latLngDataType{
      lat: number, 
      lng: number, 
      name:string
    }
    const props=defineProps<{
      fetchedLatLngData: latLngDataType| null;
    }>()

    const state = ref<'waiting'|'loading'|'fetched'>('waiting');
    let location = ref<string>('');
    let weatherData = ref({
            temp: 0,
            temp_max: 0,
            temp_min: 0,
            pressure: 0,
            humidity: 0,
            weather: 'no data',
            date: 'no data'
        })
    const forecastDays = ref([]);

   const toCelsius = (kelvin:number):number => {
      return parseFloat((kelvin-273.1).toFixed(2));
   }
   
   watch(()=>props.fetchedLatLngData, (newVal)=>callWeatherApi(newVal));
   async function callWeatherApi(newVal:latLngDataType|null) {
      if(!newVal) return; //handle null case
      location.value=newVal.name.toUpperCase();

      state.value='loading';
      //API CALL
      const response= await fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${newVal.lat}&lon=${newVal.lng}&appid=${apiKey}`)
      if(!response.ok) throw new Error('Error Fetching Weather Data');
      const data = await response.json();
      //assign data from api to weatherData
      weatherData.value = {
         temp: toCelsius(data.main.temp),
         temp_max: toCelsius(data.main.temp_max),
         temp_min: toCelsius(data.main.temp_min),
         pressure: data.main.pressure,
         humidity: data.main.humidity,
         weather: data.weather[0].main,
         date: new Date().toLocaleDateString()
      }

      //forecase APi call
      await forecastApiCall(newVal);
      state.value = 'fetched';
   }

   async function forecastApiCall(newVal: latLngDataType){
      //API CALL
      const response= await fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=${newVal.lat}&lon=${newVal.lng}&appid=${apiKey}`)
      if(!response.ok) throw new Error('Error Fetching Weather Data');
      const data = await response.json();
      //filter data having time 12:00:00 in the dt_txt key
      forecastDays.value = data.list.filter(
         (item: {dt_txt: string})=> item.dt_txt.includes("12:00:00")
      ).slice(1,4)
   }
</script>

<template>
    <div class="right-container">
        <h2>TODAY AT   {{location}}</h2>
        <div class="weather-today">
               <div class="weather-today-left" v-if="state==='fetched'">
                   <h3>{{weatherData.temp}}°</h3>
                   <P class="weather-info-para">{{weatherData.weather}}</P>
                   <p>{{weatherData.date}}</p>
               </div>
               <div class="weather-today-right" v-if="state==='fetched'">
                   <p>Temp:  {{weatherData.temp}} °C</p>
                   <p>Max Temp:  {{weatherData.temp_max}} °C</p>
                   <p>Min Temp:  {{weatherData.temp_min}} °C</p>
                   <p>Pressure:  {{weatherData.pressure}} hPa</p>
                   <p>Humidity:   {{weatherData.humidity}} %</p>
               </div>
            <p v-if="state==='loading'">Loading data from the server</p>
            <p v-if="state==='waiting'">Enter a location to view the weather</p>
        </div>
        <h2>FORECAST</h2>
        <div class="daily-forecast">
            <DailyForecast v-for="day in forecastDays" :forecast="day"/>
        </div>
        
    </div>
</template>

<style>
 .right-container{
    padding: 3rem;
    width: 50%;
    height: 100%;
 }
 .weather-today{
    background-color: var(--white-color);
    border-radius: 1rem;
    display: flex;
    flex-direction: row;
    padding: 1rem 4rem;
    justify-content: space-between;
    margin-bottom: 3rem;
    box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
 }
 .weather-today-right, .weather-today-left{
    flex: 1;
 }

 .weather-today > p{
   padding: 3rem 0;
   display: flex;
   flex-direction: row;
 }

 .weather-today-right{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    padding-left: 2rem;
    border-left: 0.1rem solid var(--text-color-black);
 }

 .weather-today-right p{
    margin: 0.5rem 0;
 }

 .weather-today-left{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
 }

 .weather-today-left h3{
    font-size: 3.1rem;
    margin: 0;
    display: flex;
    align-items: center;
 }
 .weather-today-left p{
    margin: 0.5rem 0;
 }
 .weather-info-para{
    font-size: 2rem;
    font-weight: bolder;
 }

 .daily-forecast{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: 1fr;
    gap: 1rem;
 }

@media (max-width: 1200px) {
  .right-container {
    width: 100%;
    padding: 1rem;
  }

  .weather-today {
    flex-direction: row;
    padding: 1rem;
  }

  .weather-today-left,
  .weather-today-right {
    width: 100%;
    padding: 0 3rem;
  }

  .weather-today-right {
    margin-top: 1rem;
    align-items: flex-start;
  }

  .weather-today-left h3 {
    font-size: 3rem;
  }

  .weather-info-para {
    font-size: 1.2rem;
  }
}

</style>