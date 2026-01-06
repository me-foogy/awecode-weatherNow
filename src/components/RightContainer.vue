<script setup lang="ts">
    import DailyForecast from './DailyForecast.vue';
    import {ref} from 'vue';

    let location = 'KATHMANDU';
    let weatherData = ref({
            temp: 32,
            temp_max: 33,
            temp_min: 31,
            pressure: 1015,
            humidity: 69,
            weather: 'Rain',
            date: '2026 - 2 - 3'
        })
    const forecastDays = [1,2,3];
</script>

<template>
    <div class="right-container">
        <h2>TODAY AT   {{location}}</h2>
        <div class="weather-today">
            <div class="weather-today-left">
                <h3>{{weatherData.temp}}°</h3>
                <P class="weather-info-para">{{weatherData.weather}}</P>
                <p>{{weatherData.date}}</p>
            </div>
            <div class="weather-today-right">
                <p>Temp: {{weatherData.temp}} °C</p>
                <p>Max Temp: {{weatherData.temp_max}} °C</p>
                <p>Min Temp: {{weatherData.temp_min}} °C</p>
                <p>Pressure: {{weatherData.pressure}}</p>
                <p>Humidity:  {{weatherData.humidity}}</p>
            </div>
        </div>
        <h2>FORECAST</h2>
        <div class="daily-forecast">
            <DailyForecast v-for="day in forecastDays"/>
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
    font-size: 5rem;
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