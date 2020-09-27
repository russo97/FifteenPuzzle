<template>
  <main id="app" ref="mainbox">
    <Controls
      :moves="moves"
      :playing="playing"
      :timepast="timepast"
      @new-game="newGame" />

    <WinScreen :winner="playerWins" />

    <transition-group tag="div" name="cell" id="content" :class="{ 'playerWins': playerWins }">
      <Tile
        :key="val"
        :value="val"
        :index="index"
        @move="moveCell(index)"
        v-for="(val, index) in table" />
    </transition-group>

    <Settings v-model.number="animationTime" />
  </main>
</template>

<script>
import Tile from "./components/Tile";
import Controls from "./components/Controls";
import Settings from "./components/Settings";
import WinScreen from "./components/WinScreen";

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
      animationTime: 80,
      lastKeyPressed: [],
      currentTimeStamp: 0,
      lastMovedIndexes: [],
    };
  },

  beforeMount() {
    this.table = [...this.getBlankTable(), 'EMPTY'];
  },

  mounted () {
    this.$refs.mainbox.style = `--width: ${window.innerWidth - 10}px;`;

    window.addEventListener('resize', (e) => {
      this.$refs.mainbox.style = `--width: ${e.srcElement.innerWidth - 10}px;`;
    });

    // capturando código da tecla para permitir movimento também através delas
    window.addEventListener('keyup', (e) => {
      this.lastKeyPressed.push(e.keyCode);
    }, false);
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
      const { canMove, nullIndex, playing, increaseMoves, playerWins, swap } = this;

      if (nullIndex !== index && canMove(index) && playing && !playerWins) {
        increaseMoves();

        swap(index, nullIndex);
      }
    },

    swap (indexA, indexB) {
      this.table[indexA] = this.table.splice(indexB, 1, this.table[indexA])[0];

      this.lastMovedIndexes = [indexA, indexB];
    },

    newGame () {
      const { playing, shuffleCells } = this;

      if (playing) {
        this.playing = false;

        return;
      }

      shuffleCells();
    },

    organizeTable () {
      const { nullIndex, table } = this;

      table.splice(nullIndex, 1);

      table.sort((a, b) => a - b);

      table.push('EMPTY');
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
        animationTime,
        nullAdjascentExistingIndex: adjEI,
        howManyTileAreInTheirRightPlaces: correctTiles
      } = this;

      swap(adjEI[random(adjEI.length - 1)], nullIndex);

      if (moveIndex < 40 || correctTiles > 2 || !isSolvable) {
        setTimeout(() => shuffleCells(moveIndex + 1), animationTime);
      } else {
        this.playing = true;
      }
    },

    increaseMoves () {
      this.moves += 1;
    },

    clearTimeAndMoves () {
      this.moves = 0;
      this.timepast = 0;
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
    },

    playerWins () {
      const { table, moves } = this;

      return table.slice(0, table.length - 1).every((v, i) => v === i + 1) && moves > 0;
    },

    keyMap () {
      const { nullIndex, tableSize } = this;

      return {
        '37': nullIndex + 1,
        '65': nullIndex + 1,
        '39': nullIndex - 1,
        '68': nullIndex - 1,
        '87': nullIndex + tableSize,
        '38': nullIndex + tableSize,
        '83': nullIndex - tableSize,
        '40': nullIndex - tableSize
      };
    }
  },

  watch: {
    playing (current) {
      const { clearTimeAndMoves, organizeTable } = this;

      if (current) {
        clearTimeAndMoves();
        this.currentTimeStamp = setInterval(() => this.timepast += 1, 1000);
      } else {
        if (!this.playerWins) {          
          organizeTable();
          clearTimeAndMoves();
          clearInterval(this.currentTimeStamp);
        }
      }
    },

    moves (current) {
      if (current > 0 && this.playing && this.playerWins) {
        this.playing = false;

        clearInterval(this.currentTimeStamp);
      }
    },

    lastKeyPressed (current) {
      const { table, keyMap, moveCell, lastKeyPressed } = this;

      if (current.length) {
        const index = keyMap[lastKeyPressed.splice(0, 1)[0]];

        if (Number.isInteger(index) && table[index]) {
          moveCell(index);
        }
      }
    }
  },

  components: {
    Tile,
    Controls,
    Settings,
    WinScreen
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

    .playerWins {
      filter: blur(5px);
      transition: all 300ms ease-in;
    }

    div#content {
      width: calc(100% - 6px);
      height: calc(100% - 6px);
      display: grid;
      grid-gap: 3px;
      grid-template: repeat(4, 1fr) / repeat(4, 1fr);
    }
  }

  @media screen and (max-width: 430px) {
    main#app {
      width: var(--width);
      height: var(--width);
    }
  }
</style>
