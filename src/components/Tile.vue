<template>
  <div class="cell">
      {{ value }}
  </div>
</template>

<script>
export default {
    props: ['index', 'value']
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    .cell {
        color: #fff;
        cursor: pointer;
        font-size: 150%;
        @extend %flex-center;
        background-color: #F15E5Ebd;
        font-family: 'Pacifico', cursive;
    }
</style>