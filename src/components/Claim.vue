<template>
    <div class="claimWrapper">
        <h1 class="claim">Weather app</h1>
        <p class="subclaim">Podaj nazwę miasta, w którym chcesz sprawdzić pogodę.</p>
    </div>
</template>

<script>

export default {
  name: 'Claim',
};
</script>

<style lang="scss" scoped>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }
    .claimWrapper {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: #fff;

        .claim {
            font-size: 52px;
            text-align: center;

            @media (min-width: 768px) {
                font-size: 78px;
            }

            @media (min-width: 1024px) {
                font-size: 100px;
            }
        }

        .subclaim {
            margin: 20px 0 0 0;
            font-size: 14px;
            font-weight: 400;
            text-align: center;

            @media (min-width: 768px) {
                font-size: 18px;
                width: 500px;
                letter-spacing: 1px;
                line-height: 1;
            }
        }
    }
</style>
