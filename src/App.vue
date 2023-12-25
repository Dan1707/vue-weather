<template>
  <div
    class="wrapper"
    :style="{ backgroundImage: `url(${weatherBackground})` }"
  >
    <h1 class="weather__title">Weather forecast online</h1>
    <div class="weather__search-wrapper">
      <input
        type="text"
        class="weather__search_input"
        placeholder="Type the name of your city"
        v-model="cityName"
        @keydown.enter="
          getData();
          getPhotos();
        "
        name="weather"
        id="weather"
      />
      <button
        class="weather__search_btn"
        @click="
          getData();
          getPhotos();
        "
      >
        Search
      </button>
    </div>
    <div class="weather__item-wrapper">
      <div
        class="weather__item"
        v-for="weather in weatherArray"
        :key="weather.cityName"
        @click="getWeekData(weather.cityName)"
      >
        <h3 class="weather__item_name">{{ weather.cityName }}</h3>
        <div class="weather__item_ico-wrapper">
          <img :src="weather.weatherIcon" alt="Ico" class="weather__item_ico" />
        </div>
        <h2 class="weather__item_deg">{{ weather.temperature }}C</h2>

        <h3 class="weather__item_outside">{{ weather.condition }}</h3>
        <p class="weather__item_feelslike">
          Feels like: {{ weather.feelsLike }}C
        </p>
        <button
          class="weather__item_delete-btn"
          @click.stop="deleteWeather(weather)"
        >
          Delete
        </button>
      </div>
    </div>
    <div v-if="showPopup" class="popup">
      <div class="popup__container">
        <div class="popup__exit" @click="showPopup = false">
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
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      cityName: "",
      weatherArray: [],
      weatherItemForecast: [],
      showPopup: false,
      weatherBackground: "",
    };
  },

  methods: {
    async getData() {
      try {
        const res = await fetch(
          `https://api.weatherapi.com/v1/current.json?key=8d9a41602e78462cb07132827230512&q=${this.cityName}&aqi=no`
        );

        const data = await res.json();
        console.log(data);

        const weatherData = {
          cityName: data.location.name,
          temperature: data.current.temp_c,
          condition: data.current.condition.text,
          feelsLike: data.current.feelslike_c,
          weatherIcon: data.current.condition.icon,
        };

        this.weatherArray.push(weatherData);
        console.log(this.weatherArray);
        this.cityName = "";
      } catch (error) {
        console.log(`Oh no, something went wrong..., ${error}`);
      }
    },

    async getWeekData(weatherItemName) {
      try {
        this.weatherItemForecast = [];
        const res = await fetch(
          `https://api.weatherapi.com/v1/forecast.json?key=8d9a41602e78462cb07132827230512&q=${weatherItemName}&days=7&aqi=yes&alerts=yes`
        );

        const data = await res.json();

        const weatherForecastDays = data.forecast.forecastday;

        weatherForecastDays.forEach((el) => {
          const weatherForecastItem = {
            dayName: el.date,
            weatherIcon: el.day.condition.icon,
            weatherIconText: el.day.condition.text,
            maxTemp: el.day.maxtemp_c,
            minTemp: el.day.mintemp_c,
          };

          this.weatherItemForecast.push(weatherForecastItem);
          this.showPopup = true;
        });
        console.log(this.weatherItemForecast);
      } catch (error) {
        console.log(`Oh no, something went wrong..., ${error}`);
      }
    },

    async getPhotos() {
      try {
        const res = await fetch(
          `https://api.unsplash.com/search/photos/?client_id=PXg14idWDH2ooJ01s2VZcJ85Vthonkzd4TDHSciYNN0&query=${this.cityName}`
        );
        const data = await res.json();
        this.weatherBackground = data.results[0].links.download;

        console.log(this.weatherBackground);
      } catch (error) {
        console.log(`Oh no, something went wrong..., ${error}`);
      }
    },

    deleteWeather(weatherToDelete) {
      this.weatherArray = this.weatherArray.filter(
        (el) => el !== weatherToDelete
      );
      this.weatherBackground = "";
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700&display=swap");
.wrapper,
body {
  width: 100%;
  height: 100vh;
  background-size: cover;
  background-repeat: no-repeat;
  position: relative;
}

body {
  background: linear-gradient(to right bottom, #4e8cff, #00385e);
  height: 100vh;
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  border: none;
  outline: none;
  font-family: "Open Sans", sans-serif;
}

.weather {
  &__title {
    font-weight: 700;
    font-size: 50px;
    color: #ffffff;
    text-align: center;
  }
  &__search-wrapper {
    width: 500px;
    margin: 0 auto;
    padding: 10px;
    background: #d3d3d37e;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    margin-top: 30px;
  }

  &__search {
    &_input {
      font-size: 20px;
      border-radius: 10px;
      padding: 13px;
      color: #00385e;
      transition: 0.3s ease-in;
      width: 100%;
      &::placeholder {
        color: #00385e;
      }
      &:focus {
        background-color: #ffffffc2;
      }
    }

    &_btn {
      font-size: 18px;
      padding: 13px 20px;
      background-color: #00385e;
      color: #ffffff;
      border-radius: 10px;
      font-weight: 500;
      transition: 0.3s ease-in;
      cursor: pointer;
      &:hover {
        background-color: #ffffff;
        color: #00385e;
      }
    }
  }
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
    &_name {
    }

    &_ico-wrapper {
      max-width: 50px;
      margin: 0 auto;
      margin-top: 10px;
    }

    &_ico {
      max-width: 100%;
    }

    &_delete-btn {
      position: absolute;
      width: 100%;
      height: 30px;
      text-align: center;
      font-size: 18px;
      font-weight: 500;
      background-color: #da3b3b;
      bottom: 0;
      opacity: 0;
      transition: 0.3s ease;
      cursor: pointer;
      &:hover {
        color: #ffffff;
      }
    }
  }
}

.popup {
  width: 100%;
  min-height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  overflow: hidden;
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 0px;
  &__container {
    position: relative;
    flex-basis: 100%;
    max-width: 1050px;
    background-color: #00385e;
    padding: 10px;
    border-radius: 15px;
    box-shadow: 0px 0px 10px #000;
    text-align: center;
    color: #ffffff;
    overflow: hidden;
  }
  &__title {
  }

  &__suptitle {
    color: #98abff;
  }

  &__day {
    &_wrapper {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 10px;
      align-items: center;
      justify-content: space-between;
      margin-top: 30px;
    }

    &_item {
      height: 300px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      background-color: #2770a1;
      padding: 5px;
      border-radius: 15px;
    }

    &_item-date {
      color: #98abff;
    }

    &_item-start {
      max-height: 137px;
    }

    &_item-temp {
      font-size: 12px;
    }

    &_item-maxtemp {
    }

    &_item-mintemp {
    }
  }

  &__day {
    &_item-img-wrapper {
      max-width: 70px;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-basis: 100%;
      margin: 0 auto;
      margin-top: 20px;
    }

    &_item-img {
      width: 100%;
    }
  }

  &__exit {
    position: absolute;
    top: 0;
    right: 0;
    width: 50px;
    height: 50px;
    padding: 10px;
    transition: 0.3s;
    border-radius: 15px;
    &:hover {
      background-color: #da3b3b;
      .popup__exit_btn {
        fill: #ffffff;
      }
    }
    &_btn {
      width: 100%;
      height: 100%;
    }
  }
}
</style>
