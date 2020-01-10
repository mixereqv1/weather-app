<template>
  <span :style="style" class="icon"></span>
</template>

<script>

export default {
  name: 'Icon',
  props: {
    photo: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      item: this.photo,
    };
  },
  computed: {
    style() {
      return `background-image: url('${this.photo}')`;
    },
  },
};
</script>

<style lang="scss" scoped>
    .icon {
      width: 120px;
      height: 120px;
      background-size: cover;
      background-position: center;
      background-color: gray;
      border-radius: 50%;
    }
</style>
