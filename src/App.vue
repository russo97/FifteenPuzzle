<template>
  <main id="app">
    <Controls :moves="moves" :timepast="timepast" :playing="playing" @new-game="newGame" />

    <transition-group tag="div" name="cell" id="content">
      <Tile
        :key="val"
        :value="val"
        :index="index"
        @move="moveCell(index)"
        v-for="(val, index) in table" />
    </transition-group>
  </main>
</template>

<script>
import Tile from "./components/Tile";
import Controls from "./components/Controls";

export default {
  name: 'App',

  data () {
    return {
      moves: 0,
      table: [],
      timepast: 0,
      tableSize: 4,
      timestamp: 0,
      playing: false,
      currentTimeStamp: 0,
      lastMovedIndexes: []
    };
  },

  beforeMount() {
    this.table = [...this.getBlankTable(), 'EMPTY'];
  },

  methods: {
    getBlankTable () {
      return Array.from({ length: this.realTableSize }, (item, index) => index + 1);
    },

    canMove (cellIndex) {
      const { nullIndex, tableSize: width } = this;

      return (
        (nullIndex % width !== 0         && cellIndex == nullIndex - 1) || (nullIndex + width == cellIndex) ||
        (nullIndex % width !== width - 1 && cellIndex == nullIndex + 1) || (nullIndex - width == cellIndex)
      );
    },

    moveCell (index) {
      const { canMove, nullIndex, playing, increaseMoves } = this;

      if (nullIndex !== index && canMove(index) && playing) {
        increaseMoves();

        this.swap(index, this.nullIndex);
      }
    },

    swap (indexA, indexB) {
      this.table[indexA] = this.table.splice(indexB, 1, this.table[indexA])[0];

      this.lastMovedIndexes = [indexA, indexB];
    },

    newGame () {
      this.playing = !this.playing;

      this.shuffleCells();
    },

    random (min = 0, max) {
      if (!max) [min, max] = [0, min];

      return Math.floor(Math.random() * (max - min + 1) + min);
    },

    shuffleCells (moveIndex = 0) {
      const {
        swap,
        random,
        nullIndex,
        isSolvable,
        shuffleCells,
        nullAdjascentExistingIndex: adjEI,
        howManyTileAreInTheirRightPlaces: correctTiles
      } = this;

      const index = adjEI[random(adjEI.length - 1)];

      swap(index, nullIndex);

      if (moveIndex < 80 || !isSolvable || correctTiles > 2) {
        setTimeout(() => shuffleCells(moveIndex + 1), 70);
      } else console.log(moveIndex);
    },

    increaseMoves () {
      this.moves += 1;
    }
  },

  computed: {
    isSolvable () {
      const copy = this.table.slice();

      return copy.reduce((acc, cur, i) => copy.slice(i).filter(v => cur > v).length, 0) % 2 === 0;
    },

    nullIndex () {
      return this.table.indexOf('EMPTY');
    },

    realTableSize () {
      return Math.pow(this.tableSize, 2) - 1;
    },

    howManyTileAreInTheirRightPlaces () {
      const { table } = this;

      return table.filter((v, i) => v === i + 1 && typeof v === 'number').length;
    },

    nullAdjascentExistingIndex () {
      const { nullIndex, tableSize, table, lastMovedIndexes } = this;

      const indexes = [
        nullIndex + tableSize,
        nullIndex - tableSize
      ];

      if (nullIndex % tableSize === 0) {
        indexes.push(nullIndex + 1);
      } else if (nullIndex % tableSize == tableSize - 1) {
        indexes.push(nullIndex - 1);
      } else {
        indexes.push(
          nullIndex - 1,
          nullIndex + 1
        );
      }

      return indexes.filter((idx) => table[idx] && lastMovedIndexes.indexOf(idx) < 0);
    }
  },

  components: {
    Tile,
    Controls
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
