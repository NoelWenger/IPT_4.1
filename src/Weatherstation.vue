<script setup lang="ts">
import { ref, computed } from "vue";

let Celsius = ref(0);

const Fahrenheit = computed(() => {
  return roundDecimals((Celsius.value * 9/5) + 32);
});

const Kelvin = computed(() => {
  return roundDecimals(Celsius.value + 273.15);
});

function change(tempChangeFactor) {
  Celsius.value = roundDecimals(Celsius.value + tempChangeFactor);
}

function initValues() {
  Celsius.value = getRandomTemp(-5, 25);
}

function getRandomTemp(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function roundDecimals(number) {
  return Math.floor(number * 100) / 100;
}

initValues();
</script>

<template>
  <h1>Wetterstation</h1>

  <div id="displayInfoDiv">
    <div class="tempInfo">
      <p>Temperatur in Celsius</p>
      <p>{{ Celsius }}</p>
    </div>

    <div class="tempInfo">
      <p>Temperatur in Fahrenheit</p>
      <p>{{ Fahrenheit }}</p>
    </div>

    <div class="tempInfo">
      <p>Temperatur in Kelvin</p>
      <p>{{ Kelvin }}</p>
    </div>

    <div class="tempInfo">
      <p>Gefrierpunkt unterschritten</p>
      <p v-if="Celsius < 0">Ja</p>
      <p v-else>Nein</p>
    </div>
  </div>

  <div class="buttons">
    <button @click="change(-1)">Es wird kälter</button>
    <button @click="change(1)">Es wird wärmer</button>
  </div>
</template>
