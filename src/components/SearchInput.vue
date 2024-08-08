<template>
  <div>
    <!-- search field -->
    <form>
      <div
        class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center mx-10 sm:mx-40 md:mx-40 lg:mx-80"
      >
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:outline-none focus:ring-indigo-600 focus:ring-2 ring-inset w-full lg:h-12 text-xl"
          v-model="searchTerm.query"
          @input.prevent="handleSearch()"
          onkeydown="return (event.keyCode!=13);"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg mx-20 lg:mx-80">
      <!-- si se obtuvieron respuestas de la api, se realiza el v for-->
      <div v-if="searchTerm.results && searchTerm.query != ''">
        <div v-if="searchTerm.results.length > 0">
          <div v-for="place in searchTerm.results" :key="place.id">
            <button
              @click="getWeather(place.id)"
              class="px-3 my-2 hover:text-indigo-600 hover:font-bold text-left cursor-pointer"
            >
              {{ place.name }}, {{ place.region }}, {{ place.country }}
            </button>
          </div>
        </div>
        <div v-else>
          <p>No se encontraron resultados</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
/**usamos emits para cominicarnos con el componente padre App, a traves de la funcion getWeather */
const emit = defineEmits(['place-data'])

/**Objeto reactivo a buscar en la api */
const searchTerm = reactive({
  query: '', //query a buscar
  timeout: null,
  results: [] //resultados obtenidos de la api
})

/**funcion de busqueda con delay de 500 milisegundos en cada input */
const handleSearch = () => {
  clearTimeout(searchTerm.timeout)
  searchTerm.timeout = setTimeout(async () => {
    if (searchTerm.query != '') {
      const res = await fetch(
        `http://api.weatherapi.com/v1/search.json?key=4037b40f4e7743e2bba104507241101&q=${searchTerm.query}`
      )
      const data = await res.json()
      searchTerm.results = data
      //console.log(searchTerm.results)
      //console.log(searchTerm.query)
    } else {
      searchTerm.results = null
    }
  }, 500)
}

const getWeather = async (id) => {
  //http://api.weatherapi.com/v1/forecast.json?key=4037b40f4e7743e2bba104507241101&q=London&days=3&aqi=no&alerts=no
  const res = await fetch(
    `http://api.weatherapi.com/v1/forecast.json?key=4037b40f4e7743e2bba104507241101&q=id:${id}&days=3&aqi=no&alerts=no`
  )
  const data = await res.json()

  emit('place-data', data) //enviamos al padre esa data recibida
  //console.log(data)

  //despues de 'emitir' la informacion seleccionada por el usuario, debemos limpiar el input y los datos anteriores
  searchTerm.query = ''
  searchTerm.results = null
}
</script>
