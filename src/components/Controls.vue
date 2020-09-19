<template>
    <div id="controls">
      <div class="controls_box">
        <button class="newGame">new game</button>
        <div class="controlsInfo">
            <ControlsInfo :controlinfo="timepast" title="time past" />
            <ControlsInfo :controlinfo="moves" title="moves" />
        </div>
      </div>
    </div>
</template>

<script>
import ControlsInfo from './ControlsInfo';

export default {
    name: "Controls",

    props: ['timepast', 'moves'],

    components: {
        ControlsInfo
    }
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    div#controls {
      width: 100%;
      height: 50px;
      top: -60px;
      left: 0px;
      position: absolute;
      @extend %flex-center;

      .controls_box {
        width: calc(100% - 4px);
        height: calc(100% - 4px);
        display: flex;
        @extend %flex-center-space-between;

        button.newGame {
          width: 120px;
          height: 40px;
          border: none;
          color: #fff;
          cursor: pointer;
          padding: 0 13px;
          font-size: 12pt;
          line-height: 45px;
          background: #F15E5Ebd;
          text-transform: uppercase;
          @include border-radius(3px);
          box-shadow: 0 2px 4px -1px #aaa;
          font-family: 'Overpass', sans-serif;
        }

        div.controlsInfo {
          height: 40px;
          width: calc(100% - 150px);
          @extend %flex-center-space-evenly;
        }
      }
    }
</style>