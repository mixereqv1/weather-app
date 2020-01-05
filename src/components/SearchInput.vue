<template>
    <input id="search" name="search" :value="value" @input="handleChange" />
</template>

<script>

export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>

<style lang="scss">
    input {
        margin-top: 50px;
        color: #fff;
        text-align: center;
        font-size: 18px;
        font-weight: 300;
        height: 30px;
        border: 0;
        background: none;
        border-bottom: 1px solid #fff;
        transition: box-shadow .5s ease-out;
        @media (min-width: 768px) {
          font-weight: 400;
        }
        &:focus {
          outline: none;
          box-shadow: 0 10px 20px -8px rgba(255,255,255, .5);
        }
      }
</style>
