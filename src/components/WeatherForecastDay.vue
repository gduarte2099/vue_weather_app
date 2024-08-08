<template>
  <div class="mb-2 flex items-center justify-between">
    <table class="w-full">
      <tr>
        <!-- day of the week -->
        <td class="w-1/4 text-2xl md:text-xl lg:text-3xl">
          {{ new Date(day.date).toLocaleDateString('es-es', { weekday: 'long' }) }}
        </td>

        <!-- icon -->
        <td class="w-2/4">
          <img :src="day.day.condition.icon" alt="icon" width="50" class="mx-auto" />
        </td>

        <!-- high/low temp -->
        <td class="w-1/4 text-2xl md:text-xl lg:text-3xl">
          max: {{ Math.round(day.day.maxtemp_c) }} <br> min: {{ Math.round(day.day.mintemp_c) }}
        </td>
      </tr>
    </table>
  </div>
</template>

<script setup>
defineProps({
  day: Object
})
</script>

<style scoped></style>
