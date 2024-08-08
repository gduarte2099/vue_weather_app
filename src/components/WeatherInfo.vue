<template>
  <div
    class="absolute bg-white/80 backdrop-blur-sm text-slate-900 inset-x-0 bottom-0 rounded-t-lg p-8"
  >
    <!-- Close button -->
    <div class="flex justify-end mb-10">
      <!--AQUI ENVIAMOS UN EVENTO AL PADRE, PARA QUE CIERRE LA VENTANA, PERO NO ENVIAMOS NINGUN DATO SOLO EL EVENTO-->
      <button @click="$emit('close-details')" class="p-1">
        <i class="fa-solid fa-xmark text-xl"></i>
      </button>
    </div>

    <div class="flex items-center justify-between gap-6 mb-20">
      <!-- Wind speed -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-wind mb text-2xl"></i>
        <p class="text-xl font-bold sm:text-sm">{{ place.current.wind_kph }} km/h</p>
        <p>wind</p>
      </div>
      <!-- Humidity level -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-droplet mb text-2xl"></i>
        <p class="text-xl font-bold sm:text-sm">{{ place.current.humidity }} %</p>
        <p>humidity</p>
      </div>
      <!-- Precipitation -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-umbrella mb text-2xl"></i>
        <p class="text-xl font-bold sm:text-sm">{{ place.current.precip_mm }} mm</p>
        <p v-if="screenWidth >= 360 && screenWidth <= 920">precip</p>
        <p v-else>precipitacion</p>
      </div>
    </div>
    <div class="flex items-center justify-between gap-6 mb-10">
      <!-- Wind direction -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-fan mb text-2xl"></i>
        <p class="text-xl font-bold sm:text-sm">{{ place.current.wind_dir }}</p>
        <p>direction</p>
      </div>
      <!-- Feels like -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-temperature-half mb text-2xl"></i>
        <p class="text-xl font-bold sm:text-sm">{{ Math.round(place.current.feelslike_c) }}</p>
        <p>Feels</p>
      </div>
      <!-- UV -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-sun mb text-2xl"></i>
        <p class="text-xl font-bold sm:text-sm">{{ place.current.uv }}</p>
        <p>UV index</p>
      </div>
    </div>
    <!-- Last update and delete -->
    <div class="flex justify-between items-center">
      <h3 class="text-slate-900/50 sm:text-sm">last update: {{ place.current.last_updated }}</h3>
      <!-- info tiene el boton de eliminar, asi que debe emitirlo al padre (WeatherCard) -->
      <button @click="$emit('remove-info')">
        <i class="fa-solid fa-trash"></i>
      </button>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

let screenWidth = ref(screen.width)

defineProps({
  place: Object
})

//llamamos al evento myEventHeandler cada vez que cambia la pantall
onMounted(() => {
  window.addEventListener('resize', myEventHandler)
})

onUnmounted(() => {
  window.removeEventListener('resize', myEventHandler)
})

const myEventHandler = () => {
  // your code for handling resize...
  screenWidth = ref(screen.width)
  console.log(screenWidth.value)
  //const screenHeight = ref(screen.height)
}
</script>
