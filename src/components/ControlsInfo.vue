<template>
  <div class="control_box">
      <span class="box_info">
          {{ controlinfo }}
      </span>
      <span class="box_title">
          {{ title }}
      </span>
  </div>
</template>

<script>
export default {
    name: "ControlsInfo",

    props: ['title', 'controlinfo'],

    computed: {
        formattedTime () {
            return 0;
        }
    }
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    .control_box {
        width: 40%;
        height: 100%;
        flex-direction: column;
        background: #f15e5e62;
        box-shadow: 0 2px 5px -1px #aaa;
        @extend %flex-center-space-around;
        border-bottom: solid 2px #F15E5Ebd;

        .box_info {
            height: 20px;
            font-size: 12pt;
            line-height: 25px;
            font-weight: bold;
            font-family: 'Overpass', sans-serif;
        }

        .box_title {
            color: #791d1d;
            font-family: "Segoe UI";
            text-transform: uppercase;
        }
    }
</style>