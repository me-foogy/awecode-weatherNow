<script setup lang="ts">
import { computed } from 'vue';


   interface forecastDataType{
      main: Record<string, string>,
      weather: Array<Record<string, string>>
      dt_txt: string
   }

   interface displayDataType{
      temp: number,
      weather: string,
      date: string
   }

    const props = defineProps<{
      forecast: forecastDataType
   }>()

    const toCelsius = (kelvin:number):number => {
      return parseFloat((kelvin-273.1).toFixed(2));
   }

   const forecastData ={
      temp: props.forecast.main.temp,
      weather:  props.forecast.weather[0]?.main,
      date: props.forecast.dt_txt.slice(0,10)
   }

</script>

<template>
    <div class="each-forecast">
        <h2>{{toCelsius(forecastData.temp)}}°</h2>
        <P class="weather-info">{{forecastData.weather}}</P>
        <p>{{forecastData.date}}</p>
    </div>
</template>

<style>
 .each-forecast{
    background-color: var(--white-color);
    padding: 2rem;
    border-radius: 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
 }
 .each-forecast h2{
    margin: 0.1rem 0;
    font-size: 2rem;
 }

 .each-forecast p{
    margin:0.5rem 0;
 }

 .weather-info{
    font-size: 1.5rem;
    font-weight: bolder;
 }
</style>