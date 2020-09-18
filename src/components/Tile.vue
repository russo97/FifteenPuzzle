<template>
  <div :class="{ 'cell': value }" @click="$emit('move')">
      <div class="shadow" v-if="value">{{ value }}</div>
      <div class="number" v-if="value"> {{ value }} </div>
  </div>
</template>

<script>
export default {
    name: 'Tile',

    props: ['index', 'value']
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    .cell {
        cursor: pointer;
        overflow: hidden;
        position: relative;
        @extend %flex-center;
        background-color: #F15E5Ebd;
        font-family: 'Pacifico', cursive;

        .number,
        .shadow {
            @extend %full-width, %flex-center;
            top: 0px;
            left: 0px;
            position: absolute;
        }

        .number {
            z-index: 2;
            color: #fff;
            font-size: 6rem;
            margin-top: -10px;
        }

        .shadow {
            z-index: 1;
            font-weight: bold;
            font-size: 6rem;
            margin-top: -5px;
            color: #f15e5ebd;
        }
    }
</style>