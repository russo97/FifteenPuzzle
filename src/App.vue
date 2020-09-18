<template>
  <main id="app">
    <div id="content" ref="content">
      <Tile
        :key="index"
        :value="val"
        :index="index"
        @move="moveCell(index)"
        v-for="(val, index) in table" />
    </div>
  </main>
</template>

<script>
import Tile from "./components/Tile";

export default {
  name: 'App',

  data () {
    return {
      table: [],
      tableSize: 4
    };
  },

  beforeMount() {
    this.table = [...this.getBlankTable(), null];
  },

  methods: {
    getBlankTable () {
      return Array.from({ length: this.realTableSize }, (item, index) => {
        return index + 1;
      });
    },

    canMove (cellIndex) {
      const { nullIndex, tableSize: width } = this;

      return (
        (nullIndex % width !== 0 && nullIndex - 1 == cellIndex) ||
        (nullIndex % width !== width - 1 && cellIndex == nullIndex + 1) ||
        (nullIndex + width == cellIndex) ||
        (nullIndex - width == cellIndex)
      );
    },

    moveCell (index) {
      const canMove = this.canMove(index);

      if (canMove) {
        this.swap(index, this.nullIndex);
      }
    },

    swap (indexA, indexB) {
      this.table[indexA] = this.table.splice(indexB, 1, this.table[indexA])[0];
    }
  },

  computed: {
    isSolvable () {
      const copy = this.table.slice();

      const inversionsCount = copy.reduce((acc, cur, i) => {
        return this.table.slice(i).filter(v => cur > v).length;
      }, 0);

      return inversionsCount % 2 === 0;
    },

    nullIndex () {
      return this.table.indexOf(null);
    },

    realTableSize () {
      return Math.pow(this.tableSize, 2) - 1;
    }
  },

  components: {
    Tile
  }
}
</script>

<style lang="scss">
  @import "../public/mixins.module.scss";

  main#app {
    width: 420px;
    height: 420px;
    position: relative;
    @extend %flex-center;
    background-color: #fff;
    box-shadow: 0 3px 6px -2px #aaa;

    div#content {
      width: calc(100% - 6px);
      height: calc(100% - 6px);
      display: grid;
      grid-gap: 3px;
      grid-template: repeat(4, 1fr) / repeat(4, 1fr);      
    }
  }
</style>
