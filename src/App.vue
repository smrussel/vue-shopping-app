

<template>
  <div>
    <AppHeaders/>
    <Container/>
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
import AppHeaders from './components/AppHeaders.vue';
import Container from './components/Container.vue';

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Rajdhani:wght@500&display=swap');

body{
  font-family: 'Rajdhani', sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
