<template>
  <span class="item"> {{ value }} </span>
</template>

<script>

export default {
  name: 'Item',
  props: {
    item: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      value: this.item,
    };
  },
};
</script>

<style lang="scss" scoped>
    .item {
      padding: 10px;
      margin: 5px;
      font-size: .8em;
      font-weight: bold;
      text-align: center;
    }
</style>
