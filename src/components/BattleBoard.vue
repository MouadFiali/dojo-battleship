<script>
import PlayBoard from './PlayBoard.vue';
import {generateRandomAssets} from '../services/board-helper.js';
import {shoot} from '../services/play-helper.js';
import {findTargetCell} from '../services/ia-helper.js';

export default {
  name: "BattleBoard",
  components: {
    PlayBoard,
  },
  data() {
    return {
      playerAssets: { boardCells: {}, boats: {} },
      gameStarted: false,
      humanCanPlay: false,
      IAAssets: { boardCells: {}, boats: {} },
    };
  },
  methods: {
    setAssets(target) {
      const targetRandomAssets = generateRandomAssets();
      target["boardCells"] = targetRandomAssets.boardCells;
      target["boats"] = targetRandomAssets.boats;
    },
    startGame() {
      this.setAssets(this.playerAssets);
      this.setAssets(this.IAAssets);
      this.gameStarted = true;
      this.humanCanPlay = true;
    },
    async play(cell) {
      if (this.gameStarted && this.humanCanPlay) {
        const isHumanShotAccepted = shoot(
          cell,
          this.IAAssets.boardCells,
          this.IAAssets.boats
        );
        if (isHumanShotAccepted) {
          this.humanCanPlay = false;
          await new Promise(resolve => setTimeout(resolve, 500));
          const playerTargetCell = findTargetCell(this.playerAssets.boardCells);
          shoot(
            playerTargetCell,
            this.playerAssets.boardCells,
            this.playerAssets.boats
          );
          this.humanCanPlay = true;
        }
      }
    }
  },
};

</script>

<template>
  <div class="battle-board">
    <div class="title">BattleShip</div>
    <div class="start-button-div">
      <button class="start-button"  @click="startGame">Start Game</button>
    </div>
    
    <div class="boards-container">
      <PlayBoard
        title="Player"
        :columnsCount="10"
        :rowsCount="10"
        :boardCells="playerAssets.boardCells"
      />
      <PlayBoard
        title="IA"
        :columnsCount="10"
        :rowsCount="10"
        :boardCells="IAAssets.boardCells"
        :shouldDisplayShips="false"
        @play="play"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.title {
  font-size: 30px;
  font-weight: bold;
  margin-bottom: 30px;
}

.battle-board {
  padding: 30px;
}

.boards-container {
  display: flex;
  justify-content: space-around;
  margin-bottom: 30px;
}

.start-button-div {
  display: flex;
  justify-content: center;
  align-items: center;
}

.start-button{
  font-size: 20px;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid black;
  cursor: pointer;
}
</style>