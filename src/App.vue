<template>
  <div :class="[{ flexStart: step === 1 }, 'wrapper']">
    <transition name="fade">
      <BackgroundImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1" />
    <div class="results" v-if="resultsBar && !loading && step === 1">
      <Item v-for="item in resultsBar" :item="item" :key="item[item.length-2]" />
    </div>
    <div class="results temp" v-if="resultsTemp && !loading && step === 1">
      <Icon :photo="photo" />
      <Item :item="resultsTemp" />
    </div>
    <div class="results temp" v-if="resultsTempMore && !loading && step === 1">
      <Item :item="resultsTempMore" />
    </div>
    <div class="loader" v-if="step === 1 && loading" />
    <Ad :dark="step === 1" />
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import moment from 'moment';
import BackgroundImage from './components/BackgroundImage.vue';
import Claim from './components/Claim.vue';
import SearchInput from './components/SearchInput.vue';
import Item from './components/Item.vue';
import Icon from './components/Icon.vue';
import Ad from './components/Ad.vue';

const d2d = require('degrees-to-direction');

const baseURL = 'https://api.openweathermap.org/data/2.5/weather?lang=pl&units=metric&APPID=9ae27145a55b017fd28b4bb5edabcee1&q=';
const baseImagesURL = 'http://openweathermap.org/img/wn/';

export default {
  name: 'App',
  components: {
    BackgroundImage,
    Claim,
    SearchInput,
    Item,
    Icon,
    Ad,

  },
  data() {
    return {
      searchValue: '',
      resultsBar: [],
      resultsTemp: '',
      resultsTempMore: '',
      photo: '',
      step: 0,
      loading: false,
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function() {
      this.loading = true;

      axios.get(`${baseURL}${this.searchValue}`)
        .then((response) => {
          this.loading = false;
          this.step = 1;

          this.resultsBar = [];
          this.resultsTemp = [];

          if (response.status === 200) {
            const { data } = response;

            const { visibility } = data;
            const { description, icon } = data.weather[0];

            this.photo = `${baseImagesURL}${icon}@2x.png`;

            const {
              temp, feels_like: feelsLike, temp_min: tempMin, temp_max: tempMax, pressure, humidity,
            } = data.main;
            const { speed: windSpeed, deg: windDirection } = data.wind;
            const clouds = data.clouds.all;
            let { sunrise, sunset } = data.sys;

            const time = data.dt;
            const date = new Date(time * 1000);
            const formattedDate = `Stan z: ${moment(date).format('DD/MM/YYYY HH:mm:ss')}`;

            sunrise = moment(sunrise * 1000).format('HH:mm:ss');
            sunset = moment(sunset * 1000).format('HH:mm:ss');

            const wind = `Wiatr: ${windSpeed} km/h ${d2d(windDirection)}`;
            const pressureCount = `Ciśnienie: ${pressure} hPa`;
            const cloudy = `Zachmurzenie: ${clouds}%`;
            const humidityCount = `Wilgotność: ${humidity}%`;
            const visibilityCount = `Widoczność: +/- ${visibility / 1000} km`;
            const sun = `Wschód: ${sunrise} | Zachód: ${sunset}`;

            const temperature = `${temp.toFixed(0)}°C ${description.charAt(0).toUpperCase()}${description.slice(1)}`;
            const temperatureMore = `Odczuwalna: ${feelsLike.toFixed(0)}°C  Najniższa: ${tempMin.toFixed(0)}°C Najwyższa: ${tempMax.toFixed(0)}°C`;

            this.resultsBar.push(formattedDate, wind, pressureCount,
              cloudy, humidityCount, visibilityCount, sun);
            this.resultsTemp = temperature;
            this.resultsTempMore = temperatureMore;
          }
        });
    }, 800),
  },
};
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&display=swap');
  *,
  *::before,
  *::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-family: 'Montserrat', sans-serif;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s ease;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  .wrapper {
    position: relative;
    width: 100%;
    min-height: 100vh;
    padding: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    &.flexStart {
      justify-content: flex-start;
    }

    .results {
      display: flex;
      align-items: center;
      flex-direction: column;
      width: 100%;
      margin-top: 10px;

      @media (min-width: 1024px) {
        flex-direction: row;
        align-items: center;
        justify-content: center;
      }
    }

    .temp {
      font-size: 30px;
    }
  }
  .loader {
    display: inline-block;
    width: 64px;
    height: 64px;
    margin-top: 50px;

    @media (min-width: 768px) {
      width: 90px;
      height: 90px;
    }
  }
  .loader:after {
    content: " ";
    display: block;
    width: 46px;
    height: 46px;
    margin: 8px;
    border-radius: 50%;
    border: 6px solid #1e3d4a;
    border-color: #1e3d4a transparent #1e3d4a transparent;
    animation: loader 1.2s linear infinite;

    @media (min-width: 768px) {
      width: 90px;
      height: 90px;
    }
  }
  @keyframes loader {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>
