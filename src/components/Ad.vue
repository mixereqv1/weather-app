<template>
    <a target="_blank" :class="[{ dark }, 'poweredBy']" href="https://darksky.net/poweredby/">Powered by Dark Sky</a>
</template>

<script>

export default {
  name: 'Ad',
  props: {
    dark: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
    .poweredBy {
        color: #fff;
        text-shadow: 1px 1px 5px #000;
        padding: 5px;
        position: absolute;
        bottom: 10px;
        left: 10px;
    }
    .dark {
        color: #000;
        text-shadow: none;
    }
</style>
