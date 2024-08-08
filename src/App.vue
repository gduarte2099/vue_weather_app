<template>
  <main class="mx-10 sm:mx-20 md:mx-10 ">
    <!-- DATE -->
    <div class="text-center mb-6 text-3xl sm:text-2xl md:text-2xl lg:text-2xl">
      {{
        new Date().toLocaleDateString('es-es', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>

    <!--SEARCH FIELD-->
    <div class="mb-5 overlayer">
      <!-- dentro del componente SearchInput se encuentran los datos que necesitamos-->
      <SearchInput @place-data="addPlace" />
    </div>

    <!-- WEATHER CARDS -->
    <div class="grid md:grid-cols-3 gap-2 overlayed">
      <div v-for="(place, index) in places" :key="index">
        <!-- Al llamar a deleteInfo no es necesario colocar los parentesis 'deleteInfo()', de lo contrario estara a la espera de un parametro-->
        <WeatherCard :place="place" @delete-info="deleteInfo"></WeatherCard>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import SearchInput from './components/SearchInput.vue'
import WeatherCard from './components/WeatherCard.vue'

const places = ref([])
//addPlace recibe la data desde SearchInput
const addPlace = (data) => {
  places.value.push(data)
}

//funcion que elimina el nombre del lugar dentro del array de resultados
const deleteInfo = (name) => {
  //console.log(name)
  places.value = places.value.filter(p => p.location.name !== name)
}

console.log("Desarrollado por gduarte2999@gmail.com");
</script>

<style scoped>
.overlayer{
  position: absolute;
  z-index: 2;
  width: 100%;
  left: 0;
}

.overlayed {
  position: relative;
  z-index: 1;
  margin-top: 100px;
}


</style>
