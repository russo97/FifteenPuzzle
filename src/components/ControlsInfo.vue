<template>
  <div class="control_box">
      <span class="box_info">
          {{ timetype ? formattedTime : controlinfo }}
      </span>
      <span class="box_title">
          {{ title }}
      </span>
  </div>
</template>

<script>
export default {
    name: "ControlsInfo",

    props: ['title', 'controlinfo', 'timetype'],

    methods: {
        hoursFromSecs (secs) {
            const hours = Math.floor(secs / 3600);

            return hours;
        },

        minutesFromSecs (secs) {
            const minutes = Math.floor(secs % 3600 / 60);

            return minutes;
        },

        secondsFromSecs (secs) {
            const seconds = Math.floor(secs % 3600 % 60);

            return seconds;
        }
    },

    computed: {
        formattedTime () {
            const { hoursFromSecs, minutesFromSecs, secondsFromSecs, controlinfo } = this;

            return [hoursFromSecs, minutesFromSecs, secondsFromSecs].map(mapFn => {
                return String(mapFn(controlinfo)).padStart(2, '0');
            }).join(':');
        }
    }
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    .control_box {
        width: 45%;
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