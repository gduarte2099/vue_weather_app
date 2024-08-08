<template>
  <!-- si current.is_day es 1 significa que es de dia, y si es 0, de noche-->
  <div
    class="text-white p-5 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden"
    :class="place.current.is_day === 1 ? 'bg-day' : 'bg-night'"
  >
    <!-- Location & time -->
    <div class="mb-2 flex justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl md:text-xl md:mr-2">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl md:text-xl lg:text-3xl">
          {{ new Date(place.location.localtime).getHours() }}:
          {{ new Date(place.location.localtime).getMinutes() }}
        </h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1">
      <img
        v-if="
          place.current.condition.text.toLowerCase().includes('moderate') ||
          place.current.condition.text.toLowerCase().includes('rain')
        "
        :src="place.current.condition.icon"
        alt="icon"
        width="200"
        class="mx-auto -mb-4"
      />
      <img
        v-else
        :src="place.current.condition.icon"
        alt="icon"
        width="200"
        class="mx-auto -mb-10"
      />
      <h1 class="text-9xl md:text-6xl lg:text-6xl mb-2 -mr-4">
        {{ Math.round(place.current.temp_c) }}&deg;
      </h1>
      <p
        class="text-2xl"
        :class="{
          'mb-zero':
            place.current.condition.text.toLowerCase().includes('moderate') ||
            place.current.condition.text.toLowerCase().includes('rain')
        }"
      >
        {{ place.current.condition.text }}
      </p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div v-for="(day, index) in place.forecast.forecastday" :key="index">
      <WeatherForecastDay :day="day" />
    </div>

    <!-- info -->
    <!-- aqui recibimos el emit de la funcion eliminar, y tenemos acceso al name, asi que emitimos de nuevo al padre(App) pero con el name-->
    <Transition name="fade">
      <div v-show="showDetail">
        <WeatherInfo
          :place="place"
          @close-details="showDetail = false"
          @remove-info="removeInfo(place.location.name)"
        />
      </div>
    </Transition>

    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10 text-3xl md:text-xl lg:text-2xl">
      <button @click="showDetail = true">
        More Info <i class="fa-solid fa-arrow-right -mb-px text-3xl md:text-xl lg:text-2xl"></i>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import BorderLine from './BorderLine.vue'
import WeatherForecastDay from './WeatherForecastDay.vue'
import WeatherInfo from './WeatherInfo.vue'

defineProps({
  place: Object
})

const emit = defineEmits(['delete-info'])

const showDetail = ref(false)

const removeInfo = (placeName) => {
  emit('delete-info', placeName)
  showDetail.value = false
}
</script>

<style scoped>
.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}

.bg-night {
  background-color: #07223d;
  background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.mb_zero {
  margin-bottom: -20px;
}
</style>
