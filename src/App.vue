<script setup lang="ts">
import Pokedex from './components/Pokedex.vue'
import CardsCollection from './components/CardsCollection.vue'
import Charts from './components/Charts.vue'
import axios from 'axios';
</script>

<template>
      <img src="./assets/logo.png" alt="Pokemon Logo" class="logo"/>
      <div class="logo"></div>
      <Pokedex/>
      <CardsCollection/>
      <Charts/>

</template>

<style>
@import './assets/base.css';
</style>
