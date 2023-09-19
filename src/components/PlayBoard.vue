<script>
import { getCell } from '../services/board-helper';

export default {
  name: "PlayBoard",
  props: {
    title: String,
    columnsCount: Number,
    rowsCount: Number,
    boardCells: {
      type: Object,
      default() {
        return {};
      },
    },
    shouldDisplayShips: {
      type: Boolean,
      default: true,
    },
  },
  methods:{
    getCell,
    getCellStatus(row, column){
      var cell = this.boardCells[getCell(row, column)];
      if(cell == undefined){
        return "";
      }
      return cell.status;
    }
  }
};
</script>

<template>
  <div class="playboard">
    <div class="title">{{ title }}</div>

    <div class="grid-container">
      <div v-bind:key="row" v-for="row in rowsCount" class="line">
        <div
          v-bind:key="column"
          v-for="column in columnsCount"
          class="cell"
          :class="[getCellStatus(row, column), { hidden: !shouldDisplayShips }]"
          @click="$emit('play', getCell(row, column))"
        />
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.title {
  font-size: 30px;
  font-weight: bold;
  margin-bottom: 30px;
}

.playboard {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.line {
  margin: 0px;
  width: fit-content;
  display: flex;
  border-top: 1px solid black;
  border-bottom: 1px solid black;

  &+& {
    border-top: none;
  }
}

.cell {
  border-left: 1px solid black;
  border-right: 1px solid black;

  &+& {
    border-left: none;
  }

  height: 3vw;
  width: 3vw;
}
.ship {
  background-color: black;
}
.ship.hidden {
  background-color: unset;
}
.missed {
  background-color: aqua;
}
.hit {
  background-color: yellow;
}
.sunk {
  background-color: red;
}
.boards-container {
  display: flex;
  justify-content: space-around;
  margin-bottom: 30px;
}
</style>
```

