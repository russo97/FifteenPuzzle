<template>
  <main id="app">
    <div id="content">
      <div
        :key="index"
        class="cell"
        v-for="(n, index) in 16">
        {{ n }}
      </div>
    </div>
  </main>
</template>

<script>

export default {
  name: 'App',

  data () {
    return {};
  },

  components: {}
}
</script>

<style lang="scss">
  main#app {
    width: 420px;
    height: 420px;
    display: flex;
    position: relative;
    align-items: center;
    justify-content: center;
    background-color: #fff;
    box-shadow: 0 3px 6px -2px #aaa;

    div#content {
      width: calc(100% - 6px);
      height: calc(100% - 6px);
      display: grid;
      grid-gap: 3px;
      grid-template: repeat(4, 1fr) / repeat(4, 1fr);

      .cell {
        background-color: #F15E5Ebd;
      }
    }
  }
</style>
