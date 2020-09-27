<template>
  <div id="winScreen" :class="{ 'winner': winner }" @click="evt => winner = !winner">
      <div class="winnerBox" :class="{ 'winner': winner }">
          amazing
      </div>
  </div>
</template>

<script>
export default {
    name: 'WinScreen',

    props: ['winner'],

    data () {}
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    #winScreen {
        z-index: 6;
        top: 0px;
        left: -100%;
        width: 100%;
        height: 100%;
        position: absolute;
        @extend %flex-center-center;

        &.winner {
            left: 0px;
        }

        .winnerBox {
            opacity: 0;
            width: 75%;
            height: 50%;
            margin-top: 40px;
            background: white;
            box-shadow: 0 2px 5px 5px #fff;
            transition: all 500ms ease-in-out 300ms;

            &.winner {
                opacity: 1;
                margin-top: -20px;
            }
        }
    }
</style>