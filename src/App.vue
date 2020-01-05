<template>
  <div class="wrapper">
    <SearchInput v-model="searchValue" @input="handleInput" />

  </div>
</template>

<script>
// import openGeocoder from 'node-open-geocoder';
import debounce from 'lodash.debounce';
import SearchInput from './components/SearchInput.vue';

// openGeocoder()
//   .geocode('Katowice')
//   .end((err, res) => {
//     if (err) console.log(err);
//     console.log(res[0]);
//   });

export default {
  name: 'App',
  components: {
    SearchInput,

  },
  data() {
    return {
      searchValue: '',
      result: [],
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function() {
      console.log(this.searchValue);
    }, 500),
  },
};
</script>

<style lang="scss">
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
  .wrapper {
    position: relative;
    width: 100%;
    min-height: 100vh;
    padding: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: #141414;
  }
</style>
