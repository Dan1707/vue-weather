<template>
  <div class="weather__item">
    <h3 class="weather__item_name">{{ weather.cityName }}</h3>
    <div class="weather__item_ico-wrapper">
      <img :src="weather.weatherIcon" alt="Ico" class="weather__item_ico" />
    </div>
    <h2 class="weather__item_deg">{{ weather.temperature }}C</h2>

    <h3 class="weather__item_outside">{{ weather.condition }}</h3>
    <p class="weather__item_feelslike">Feels like: {{ weather.feelsLike }}C</p>
    <button
      class="weather__item_delete-btn"
      @click.stop="deleteWeather(weather)"
    >
      Delete
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },

  props: {
    weather: Object,
  },
};
</script>

<style lang="scss" scoped>
.weather {
  &__item-wrapper {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat (auto, 250px);
    justify-content: space-between;
    align-items: center;
    max-width: 800px;
    gap: 20px;
    margin: 0 auto;
    margin-top: 50px;
  }
  &__item {
    padding: 20px 20px 50px 20px;
    border-radius: 15px;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #2770a1;
    color: #ffffff;
    text-align: center;
    position: relative;
    box-shadow: 0px 0px 40px -7px rgba(0, 0, 0, 0.75);
    overflow: hidden;
    cursor: pointer;
    &:hover {
      .weather__item_delete-btn {
        opacity: 1;
      }
    }
  }
}
</style>
