<template>
  <div class="settings">
      settings
  </div>
</template>

<script>
export default {
 name: 'Setting',

 props: []
}
</script>

<style lang="scss">
    @import "../../public/mixins.module.scss";

    .settings {
        width: 100%;
        height: 100%;
        top: 0px;
        left: 0px;
        z-index: -1;
        position: absolute;
    }
</style>