<template>
  <div :class="[{ flexStart: step === 1 }, 'wrapper']">
    <transition name="fade">
      <BackgroundImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1" />
    <div class="results" v-if="results && !loading && step === 1">
      <Item v-for="item in results" :item="item" :key="item.id" />
    </div>
    <div class="loader" v-if="step === 1 && loading" />
    <Ad :dark="step === 1" />
  </div>
</template>

<script>
// import openGeocoder from 'node-open-geocoder';
import axios from 'axios';
import debounce from 'lodash.debounce';
// import moment from 'moment';
import BackgroundImage from './components/BackgroundImage.vue';
import Claim from './components/Claim.vue';
import SearchInput from './components/SearchInput.vue';
import Item from './components/Item.vue';
import Ad from './components/Ad.vue';

// const baseURL = 'https://api.darksky.net/forecast/88fbd9acbd7f7c461bd125ee696060a8/';
const baseURL = 'https://api.openweathermap.org/data/2.5/weather?lang=pl&units=metric&APPID=9ae27145a55b017fd28b4bb5edabcee1&q=';

export default {
  name: 'App',
  components: {
    BackgroundImage,
    Claim,
    SearchInput,
    Item,
    Ad,

  },
  data() {
    return {
      searchValue: '',
      results: [],
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
          // console.log(response);

          if (response.status === 200) {
            const { data } = response;
            console.log(data);

            // eslint-disable-next-line no-unused-vars
            const { description } = data.weather[0];
            const {
            // eslint-disable-next-line
              temp, feels_ike, temp_min, temp_max, pressure, hummidity
            } = data.main;
            // eslint-disable-next-line no-unused-vars
            const { speed, deg } = data.wind;
            // eslint-disable-next-line no-unused-vars
            const { sunrise, sunset } = data.sys;
          }
        });

      // openGeocoder()
      //   .geocode(this.searchValue)
      //   .end((err, result) => {
      //     axios.get(`${baseURL}${result[0].lat},${result[0].lon}?lang=pl&units=ca`)
      //       .then((response) => {
      //         this.loading = false;
      //         this.step = 1;

      //         const currentlyData = response.data.currently;

      //         const {
      //           // eslint-disable-next-line no-unused-vars
      //           time, summary, temperature, apparentTemperature, pressure, windSpeed, uvIndex,
      //         } = currentlyData;

      //         const date = new Date(time * 1000);
      //         const formattedDate = moment(date).format('DD/MM/YYYY HH:mm:ss');

      //         this.results = [
      //           {
      //             id: 1, name: 'Data: ', value: formattedDate, unit: '',
      //           },
      //           {
      //             id: 2, name: 'Niebo: ', value: summary, unit: '',
      //           },
      //           {
      //             id: 3, name: 'Temperatura: ', value: Math.round(temperature), unit: '°C',
      //           },
      //           {
      //             id: 4, name: 'Odczuwalna temperautura: ', value:
      // Math.round(apparentTemperature), unit: '°C',
      //           },
      //           {
      //             id: 5, name: 'Ciśnienie: ', value: pressure, unit: 'hPa',
      //           },
      //           {
      //             id: 6, name: 'Wiatr: ', value: windSpeed, unit: 'km/h',
      //           },
      //           {
      //             id: 7, name: 'Indeks UV: ', value: uvIndex, unit: '',
      //           },
      //         ];

      //         console.log(this.results);
      //       });
      //   });
    }, 500),
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
