<script setup lang="ts">

</script>

<template>
    <div class="each-forecast">
        <h2>33°</h2>
        <P class="weather-info">Sunny</P>
        <p>2026-1-3</p>
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
    font-size: 3rem;
 }

 .each-forecast p{
    margin:0.5rem 0;
 }

 .weather-info{
    font-size: 1.5rem;
    font-weight: bolder;
 }
</style>