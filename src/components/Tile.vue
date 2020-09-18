<template>
  <div :class="{'filled': valid}" class="cell move-item" @click="$emit('move')">
      <div class="shadow" v-if="valid">{{ value }}</div>
      <div class="number" v-if="valid"> {{ value }} </div>
  </div>
</template>

<script>
export default {
    name: 'Tile',

    props: ['index', 'value'],

    computed: {
        valid () {
            return typeof this.value !== 'string';
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
            transition: all 300ms cubic-bezier(0,.88,.44,1.3);
        }

        &.filled {
            cursor: pointer;
            background-color: #F15E5Ebd;
        }

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