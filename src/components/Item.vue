<template>
    <div class="item">
      <span> {{ name }} </span>
      <span> {{ value }} </span>
      <span> {{ unit }} </span>
    </div>
</template>

<script>

export default {
  name: 'Item',
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      name: this.item.name,
      value: this.item.value,
      unit: this.item.unit,
    };
  },
};
</script>

<style lang="scss" scoped>
    .item {
        padding: 20px;
        margin: 10px;
        background: #000;
        color: #fff;
        width: 250px;
        // width: 90%;
        // height: 400px;

        // @media (min-width: 768px) {
        //     width: 80%;
        // }
    }
</style>
