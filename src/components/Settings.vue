<template>
  <div class="settings">
      <div class="togglebox"></div>
      <div class="togglebox">
        <input type="checkbox" id="slowMotionAnimated" v-model="timing" true-value="500" false-value="80" />
        <label for="slowMotionAnimated">shuffle slowly</label>
      </div>
  </div>
</template>

<script>
export default {
 name: 'Setting',

 data () {
     return {
         timing: 0
     }
 },

 watch: {
     timing (current) {
         this.$emit('input', current);
     }
 },

 props: ['animationTime']
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    .settings {
        width: 100%;
        height: 50px;
        left: 0px;
        bottom: -50px;
        position: absolute;
        @extend %flex-center-space-between;

        .togglebox {
            input[type="checkbox"] {
                display: none;

                &:checked + label {
                    &:after,
                    &:before {
                        width: 100%;
                        height: 110%;
                    }
                }
            }

            label {
                cursor: pointer;
                font-size: .9rem;
                margin-right: 2px;
                padding: 5px 8px;
                color: #f15e5e;
                font-weight: bold;
                position: relative;
                text-shadow: 0 2px 1px 2px #aaa;
                text-transform: uppercase;
                font-family: 'Overpass', sans-serif;

                &:after,
                &:before {
                    content: '';
                    width: 0px;
                    height: 110%;
                    top: -10%;
                    position: absolute;
                    background: #f15e5e2c;
                }

                &:before {
                    left: 0px;
                    transition: width 250ms ease-in;
                }

                &:after  {
                    right: 0px;
                    transition: width 250ms ease-in 250ms;
                }
            }
        }
    }
</style>