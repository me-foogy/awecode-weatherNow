<script setup lang="ts">
</script>

<template>
    <div class="left-image">
        <h1>Weather <br> Now</h1>
        <div class="search-container">
          <form>
            <label for="search-text"></label><br>
            <div class="search-bar">
              <input type="text" placeholder="Enter Location" id="search-text" aria-label="Location Input">
              <button type="submit" class="button-design">Search</button>
            </div>
          </form>
          <div class="quick-search">
            <p>Select a location for quick search</p>
            <span>
              <button class="quick-search-button">Kathmandu</button>
              <button class="quick-search-button">Pokhara</button>
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

.quick-search{
  padding: 4rem 0;
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

@media (max-width: 1200px) {
  .left-image {
    width: 100%;
    padding: 1.5rem;
    height: 100vh;
  }

  .left-image h1 {
    font-size: 4rem;
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
  }

  .search-bar input{
    padding: 0.5rem;
  }

  .quick-search {
    padding: 2rem 0;
  }

  .quick-search span {
    flex-direction: row;
    gap: 2rem;
  }

  .quick-search p {
    font-size: 1rem;
  }
}

</style>