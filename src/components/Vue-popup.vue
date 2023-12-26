<template>
  <div class="popup__container">
    <div class="popup__exit" @click="$emit('close')">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        x="0px"
        y="0px"
        class="popup__exit_btn"
        viewBox="0 0 50 50"
      >
        <path
          d="M 25 2 C 12.309534 2 2 12.309534 2 25 C 2 37.690466 12.309534 48 25 48 C 37.690466 48 48 37.690466 48 25 C 48 12.309534 37.690466 2 25 2 z M 25 4 C 36.609534 4 46 13.390466 46 25 C 46 36.609534 36.609534 46 25 46 C 13.390466 46 4 36.609534 4 25 C 4 13.390466 13.390466 4 25 4 z M 32.990234 15.986328 A 1.0001 1.0001 0 0 0 32.292969 16.292969 L 25 23.585938 L 17.707031 16.292969 A 1.0001 1.0001 0 0 0 16.990234 15.990234 A 1.0001 1.0001 0 0 0 16.292969 17.707031 L 23.585938 25 L 16.292969 32.292969 A 1.0001 1.0001 0 1 0 17.707031 33.707031 L 25 26.414062 L 32.292969 33.707031 A 1.0001 1.0001 0 1 0 33.707031 32.292969 L 26.414062 25 L 33.707031 17.707031 A 1.0001 1.0001 0 0 0 32.990234 15.986328 z"
        ></path>
      </svg>
    </div>
    <h2 class="popup__title">Weeekly weather forecast</h2>
    <div class="popup__day_wrapper">
      <div
        class="popup__day_item"
        v-for="day in weatherItemForecast"
        :key="day.dayName"
      >
        <div class="popup__day_item-start">
          <h3 class="popup__day_item-date">{{ day.dayName }}</h3>
          <div class="popup__day_item-img-wrapper">
            <img :src="day.weatherIcon" class="popup__day_item-img" />
          </div>
          <h4 class="popup__day_item-suptitle">
            {{ day.weatherIconText }}
          </h4>
        </div>
        <p class="popup__day_item-maxtemp">Max temp: {{ day.maxTemp }}</p>
        <p class="popup__day_item-mintemp">Min temp: {{ day.minTemp }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    weatherItemForecast: Array,
  },
  emits: ["close"],
};
</script>

<style lang="scss" scoped></style>
