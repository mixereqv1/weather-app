<template>
    <div class="heroImage"></div>
</template>

<script>

export default {
  name: 'BackgroundImage',

};
</script>

<style lang="scss">
    .heroImage {
        position: fixed;
        top: 0;
        z-index: -1;
        width: 100%;
        height: 100vh;
        background: url('../assets/backgroundImage.jpg') no-repeat;
        background-size: cover;
        background-position: 50% 0%;

        &::after {
            content: '';
            position: absolute;
            background: rgba(0,0,0, .3);
            top: 0;
            left: 0;
            z-index: 1;
            width: 100%;
            height: 100vh;
        }
    }
</style>
