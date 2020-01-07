<template>
  <div :class="[{ flexStart: step === 1 }, 'wrapper']">
    <transition name="fade">
      <BackgroundImage v-if="step === 0" />
    </transition>
    <Claim v-if="step === 0" />
    <SearchInput v-model="searchValue" @input="handleInput" />

    <Ad />
  </div>
</template>

<script>
import openGeocoder from 'node-open-geocoder';
import axios from 'axios';
import debounce from 'lodash.debounce';
// import moment from 'moment';
import SearchInput from './components/SearchInput.vue';
import BackgroundImage from './components/BackgroundImage.vue';
import Ad from './components/Ad.vue';
import Claim from './components/Claim.vue';

const baseURL = 'https://api.darksky.net/forecast/88fbd9acbd7f7c461bd125ee696060a8/';

export default {
  name: 'App',
  components: {
    SearchInput,
    BackgroundImage,
    Ad,
    Claim,

  },
  data() {
    return {
      searchValue: '',
      result: [],
      step: 0,
      loading: false,
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function() {
      openGeocoder()
        .geocode(this.searchValue)
        .end((err, result) => {
          axios.get(`${baseURL}${result[0].lat},${result[0].lon}?lang=pl&units=ca`)
            .then((response) => {
              const currentlyData = response.data.currently;
              console.log(currentlyData);
              // const date = new Date(currentlyData.time * 1000);
              // const formattedDate = moment(date).format('DD/MM/YYYY');

              const {
                // eslint-disable-next-line no-unused-vars
                time, summary, temperature, apparentTemperature, pressure, windSpeed, uvIndex,
              } = currentlyData;

              console.log(`Wiatr: ${Math.round(windSpeed)}km/h`);
            });
        });
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
