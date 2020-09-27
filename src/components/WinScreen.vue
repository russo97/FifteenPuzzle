<template>
  <div id="winScreen" :class="{ 'winner': winner }">
      <div class="winnerBox" :class="{ 'winner': winner }">
          <h1>Amazing</h1>

          <p>you did it</p>
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
        transition: all 0ms ease 500ms;

        &.winner {
            left: 0px;
            transition: all 0ms ease 0ms;
        }

        .winnerBox {
            opacity: 0;
            width: 75%;
            height: 50%;
            margin-top: 40px;
            flex-direction: column;
            @include border-radius(3px);
            box-shadow: 0 2px 4px #F15E5E;
            @extend %flex-center-space-evenly;
            background: rgba(255, 255, 255, 0.811);
            transition: all 500ms ease-in-out 0ms;

            &.winner {
                opacity: 1;
                margin-top: -20px;
                transition: all 500ms ease-in-out 0ms;
            }

            h1 {
                color: #F15E5E;
                font-size: 2.4rem;
                font-family: Pacifico;
                text-transform: uppercase;
                text-shadow: 0 2px 4px #F15E5E;
            }

            p {
                color: #F15E5E;
                font-size: 1.4rem;
                font-family: "Overpass";
            }
        }
    }
</style>