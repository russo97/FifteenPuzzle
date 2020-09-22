<template>
  <div :class="[{ 'filled': valid }, { 'correctPosition': correctPosition }]" class="cell move-item" @click="$emit('move')">
      <div class="shadow" v-if="valid">{{ value }}</div>
      <div class="number" v-if="valid"> {{ value }} </div>
      <div class="ball1" v-if="valid"></div>
      <div class="ball2" v-if="valid"></div>
  </div>
</template>

<script>
export default {
    name: 'Tile',

    props: ['index', 'value'],

    computed: {
        valid () {
            return typeof this.value !== 'string';
        },

        correctPosition () {
            const { index, value } = this;

            return index === value - 1;
        }
    }
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    .cell {
        overflow: hidden;
        position: relative;
        @extend %flex-center;
        font-family: 'Pacifico', cursive;

        &-#{move} {
            transition: all 50ms ease-in-out;
        }

        &.filled {
            cursor: pointer;
            background-color: #F15E5Ebd;
        }

        &.correctPosition {
            background-color: #f15e5ed4;
        }

        .number,
        .shadow {
            @extend %full-width, %flex-center;
            top: 0px;
            left: 0px;
            font-size: 4rem;
            position: absolute;
        }

        .number {
            z-index: 2;
            color: #fff;
            margin-top: -10px;
        }

        .shadow {
            z-index: 1;
            margin-top: -5px;
            font-weight: bold;
            color: #f15e5ebd;
        }


        .ball1,
        .ball2 {
            position: absolute;
            @include border-radius(50%);
        }

        .ball1 {
            width: 75%;
            height: 75%;
            top: 8%;
            left: 8%;
            background-color: #f15e5e2b;
        }

        .ball2 {
            width: 50%;
            height: 50%;
            top: 40%;
            left: 40%;
            background-color: #f15e5e4a;
        }
    }
</style>