<script setup lang="ts">

  import {computed, ref} from 'vue';
  const location = ref<string>('');
  const apiKey = import.meta.env.VITE_GEOCODING_API;
  import { useRouter } from 'vue-router'
  const router = useRouter()

  interface geolocatorApiResponse{
    country: string,
    lat: number, 
    lon: number,
    name: string,
    local_names: Record<string, string>
  }

  interface emitType {
    lat: number
    lng: number
    name: string
  }
  const latLngData = ref<emitType | null>(null);
  
  const emit = defineEmits<{
    (e: 'return-lat-lng', latLngData: emitType):void ;
  }>()

  const regexCheck = /^[A-Za-z\s]*$/; //only english characters allowed
  //user Input Check block
   const isInputValid = computed<boolean>(()=>{
    return regexCheck.test(location.value);
   })
 
  //api call to fetch location - Geolocator Api Call
  async function fetchLatLng(): Promise<geolocatorApiResponse | null> {
        try{
          let response = await fetch(`https://api.openweathermap.org/geo/1.0/direct?q=${location.value}&limit=1&appid=${apiKey}`);
          if(!response.ok) throw new Error('Unable to fetch data from the server');
          const data: Array<geolocatorApiResponse> = await response.json();
          // handleing no data cases
          if(data.length == 0){
            alert('No data found for such location');
            console.warn('No data found for such location');
            return null;
          }
          if(!data[0]) return null;
          return data[0];
        }
        catch(err){
          alert(err);
          console.error('Error Fetching lat and lng data:', err);
          return null;
        }
    }

  const handleSubmit = async () =>{
    if(!isInputValid.value) return;
    //actual APi call to geocoding api
    const fetchedData:geolocatorApiResponse | null = await fetchLatLng();
    if(!fetchedData) return;
    latLngData.value = {
      lat: fetchedData.lat,
      lng: fetchedData.lon,
      name: fetchedData.name
    }
    router.push({
      name: 'home',
      query: {
        lat: latLngData.value.lat,
        lng: latLngData.value.lng,
        name: latLngData.value.name
      }
     })
    emit('return-lat-lng', latLngData.value);
  }

  const searchKathmandu = () =>{
    latLngData.value = {
      lat: 27.71,
      lng: 85.32,
      name: 'kathmandu'
    }
    router.push({
      name: 'home',
      query: {
        lat: latLngData.value.lat,
        lng: latLngData.value.lng,
        name: latLngData.value.name
      }
     })
    emit('return-lat-lng', latLngData.value);
  }

  const searchPokhara = () =>{
    latLngData.value = {
      lat: 28.20,
      lng: 83.98,
      name: 'pokhara'
    }
    router.push({
      name: 'home',
      query: {
        lat: latLngData.value.lat,
        lng: latLngData.value.lng,
        name: latLngData.value.name
      }
     })
    emit('return-lat-lng', latLngData.value);
  }
  
</script>

<template>
    <div class="left-image">
        <h1>Weather <br> Now</h1>
        <div class="search-container">
          <form @submit.prevent="handleSubmit">
            <label for="search-text"></label><br>
            <div class="search-bar">
              <input required v-model="location" type="text" placeholder="Enter Location" id="search-text" aria-label="Location Input">
              <button type="submit" class="button-design">Search</button>
            </div>
            <p v-show="!isInputValid">no numbers and special characters allowed</p>
          </form>
          <div class="quick-search">
            <p>Select a location for quick search</p>
            <span>
              <button class="quick-search-button" @click="searchKathmandu">Kathmandu</button>
              <button class="quick-search-button" @click="searchPokhara">Pokhara</button>
            </span>
          </div>
        </div>
    </div>
</template>

<style scoped>

.left-image{
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  height: 100%;
  width: 50%;
  border-radius: 1rem;
  background-image: url('../assets/weather-bg.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  position: relative;
}
.left-image::before{
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0) 20%,
    rgba(0, 0, 0, 0.4) 60%,
    rgba(0, 0, 0, 0.8) 100%
  );
  z-index: 0;
  border-radius: 1rem;
}
.left-image >*{
  position: relative;
  z-index: 1;
}

.left-image h1{
  font-size: 5rem;
  margin: 1rem;
  color: var(--text-color-white);
}

.search-container{
  display: flex;
  flex-direction: column;
  margin-top: 2rem;
  align-items: center;
  flex: 1;
  width: 100%;
}

.search-container form{
  width: 100%;
}

.search-bar{
  display: flex;
  flex-direction: row;
  padding: 0 3rem;
  width: 100%;
  gap: 0.2rem;
}

.search-bar input{
  flex: 1;
  border-radius: 0.2rem;
  padding-left: 1rem;
  background-color: rgba(0,0,0,0.4);
  color: var(--text-color-white);
  border: 0.1rem solid var(--text-color-white);
}

.search-bar input::placeholder{
  color: var(--text-color-white);
}

form p {
  color: rgb(249, 41, 41);
  padding: 0 3rem;
}

.quick-search{
  padding: 2rem 0;
}
.quick-search p{
  color: var(--text-color-white);
  padding-bottom: 1rem;
  text-align: center;
}
.quick-search span{
  display: flex;
  flex-direction: row;
  gap: 2rem;
  justify-content: center;
  align-items: center;
}

@media (max-width: 900px) {
  .left-image {
    width: 100%;
    padding: 1.5rem;
    height: 100vh;
  }

  .left-image h1 {
    font-size: 6rem;
    margin: 0.5rem 0;
    text-align: center;
  }

  .search-bar {
    margin-top: 2rem;
    flex-direction: column;
    gap: 1rem;
    padding: 0rem 2rem;
  }

   .search-bar input, .search-bar button {
    width: 100%;
    margin: 1rem 0;
  }

  .search-bar input{
    padding: 0.5rem;
  }

  .quick-search {
    padding: 5rem 0;
  }

  .quick-search span {
    flex-direction: row;
    gap: 2rem;
  }

  .quick-search p {
    font-size: 1.2rem;
  }
}

</style>