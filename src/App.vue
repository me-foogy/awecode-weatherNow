<script setup lang="ts">
import RightContainer from './components/RightContainer.vue';
import SearchContainer from './components/SearchContainer.vue';


</script>
<template>
  <div class="center-everything">
    <div class="main-container">
      <SearchContainer/>
      <RightContainer/>
    </div>
  </div>
</template>

<style scoped>
.center-everything{
  background-color: var(--text-color-black);
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.main-container{
  background-color: var(--background-color);
  width: 90%;
  height: 90%;
  border-radius: 1rem;
  display: flex;
  flex-direction: row;
  gap: 1rem;
}

@media (max-width: 1200px) {
  .center-everything{
    height: auto;
    padding-top: 2rem;
  }

  .main-container {
    height: auto;
    flex-direction: column;
  }
}


</style>
