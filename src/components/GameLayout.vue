<template>
  <div class="layout">
    <h2 class="result">{{ computedResult }}</h2>
    <div class="wrapper">
      <div class="container">
        <div>
          <div class="grid-wrapper top">
            <div class="grid grid-11" @click="handleGridClick('0')">
              <component
                v-if="gridData[0]"
                :is="gridData[0] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
            <div class="grid grid-12" @click="handleGridClick('1')">
              <component
                v-if="gridData[1]"
                :is="gridData[1] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
            <div class="grid grid-13" @click="handleGridClick('2')">
              <component
                v-if="gridData[2]"
                :is="gridData[2] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
          </div>

          <div class="grid-wrapper middle">
            <div class="grid grid-21" @click="handleGridClick('3')">
              <component
                v-if="gridData[3]"
                :is="gridData[3] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
            <div class="grid grid-22" @click="handleGridClick('4')">
              <component
                v-if="gridData[4]"
                :is="gridData[4] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
            <div class="grid grid-23" @click="handleGridClick('5')">
              <component
                v-if="gridData[5]"
                :is="gridData[5] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
          </div>

          <div class="grid-wrapper bottom">
            <div class="grid grid-31" @click="handleGridClick('6')">
              <component
                v-if="gridData[6]"
                :is="gridData[6] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
            <div class="grid grid-32" @click="handleGridClick('7')">
              <component
                v-if="gridData[7]"
                :is="gridData[7] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
            <div class="grid grid-33" @click="handleGridClick('8')">
              <component
                v-if="gridData[8]"
                :is="gridData[8] === 'x' ? 'IconX' : 'IconO'"
              />
            </div>
          </div>
        </div>

        <div class="player-text-wrapper">
          <h5 class="player-text">Player 1 (x)</h5>
          <h5 class="player-text">Tie</h5>
          <h5 class="player-text">Player 2 (o)</h5>
        </div>

        <div class="score-text-wrapper">
          <h5 class="score-text">{{ p1Score }}</h5>
          <h5 class="score-text">{{ tieScore }}</h5>
          <h5 class="score-text">{{ p2Score }}</h5>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import IconX from "./icons/IconX.vue";
import IconO from "./icons/IconO.vue";
export default {
  components: {
    IconX,
    IconO,
  },
  data() {
    return {
      gridData: [null, null, null, null, null, null, null, null, null],
      activePlayer: "",
      p1: "x",
      p2: "o",
      p1Score: 0,
      p2Score: 0,
      tieScore: 0,
      hasWon: false,
      isTie: false,
    };
  },
  computed: {
    computedResult() {
      if (this.isTie) {
        return "There has been a tie... 🤗";
      }
      if (this.hasWon) {
        return `${
          this.activePlayer === this.p1 ? "PLAYER 1" : "PLAYER 2"
        } has won! 🎉🥂🥳`;
      }
      if (this.activePlayer) {
        return `It's ${
          this.activePlayer === this.p1 ? "PLAYER 1's 😎" : "PLAYER 2's 🤓"
        } turn`;
      }
    },
  },
  mounted() {
    this.activePlayer = this.p1;
  },
  methods: {
    handleGridClick(gridIndex) {
      if (this.hasWon || this.isTie) {
        this.resetGame();
        return;
      }

      if (this.gridData[gridIndex]) {
        return;
      }

      this.$set(this.gridData, gridIndex, this.activePlayer);
      this.hasWon = this.checkForWin(this.activePlayer);
      this.isTie = this.checkForTie();
      if (this.isTie) {
        this.tieScore++;
        return;
      }
      if (this.hasWon) {
        this.activePlayer === this.p1 ? this.p1Score++ : this.p2Score++
      } else {
        this.switchActivePlayer();
      }
    },

    switchActivePlayer() {
      if (this.activePlayer === this.p1) {
        this.activePlayer = this.p2;
      } else if (this.activePlayer === this.p2) {
        this.activePlayer = this.p1;
      }
    },

    checkForWin(playerValue) {
      if (
        this.gridData[0] === playerValue &&
        this.gridData[1] === playerValue &&
        this.gridData[2] === playerValue
      ) {
        return true;
      } else if (
        this.gridData[3] === playerValue &&
        this.gridData[4] === playerValue &&
        this.gridData[5] === playerValue
      ) {
        return true;
      } else if (
        this.gridData[6] === playerValue &&
        this.gridData[7] === playerValue &&
        this.gridData[8] === playerValue
      ) {
        return true;
      } else if (
        this.gridData[0] === playerValue &&
        this.gridData[3] === playerValue &&
        this.gridData[6] === playerValue
      ) {
        return true;
      } else if (
        this.gridData[1] === playerValue &&
        this.gridData[4] === playerValue &&
        this.gridData[7] === playerValue
      ) {
        return true;
      } else if (
        this.gridData[2] === playerValue &&
        this.gridData[5] === playerValue &&
        this.gridData[8] === playerValue
      ) {
        return true;
      } else if (
        this.gridData[0] === playerValue &&
        this.gridData[4] === playerValue &&
        this.gridData[8] === playerValue
      ) {
        return true;
      } else if (
        this.gridData[2] === playerValue &&
        this.gridData[4] === playerValue &&
        this.gridData[6] === playerValue
      ) {
        return true;
      } else {
        return false;
      }
    },

    checkForTie() {
      return !this.hasWon && !this.gridData.includes(null);
    },

    resetGame() {
      this.gridData = [null, null, null, null, null, null, null, null, null];
      this.hasWon = false;
      this.isTie = false;
      this.switchActivePlayer();
    },
  },
};
</script>

<style scoped>
.layout {
  height: 90vh;
}
.wrapper {
  height: 90vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container {
  /*height: 600px;*/
  /*width: 600px;*/
  /*background-color: red;*/
  /*place-items: center;*/
  /*display: flex;*/
}
.result {
  position: absolute;
  font-size: 36px;
  line-height: 50px;
  text-align: center;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
}

.grid-wrapper {
  display: flex;
}

.grid {
  /*border: 3px solid white;*/
  width: 200px;
  height: 200px;
  border-left: 0 !important;
  border-color: white;
  border-style: solid;
  cursor: pointer;

  /* text */
  font-size: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  line-height: 0;
}

.grid svg {
  width: 134px;
  height: 134px;
  color: white;
}
.grid svg.x-icon {
  width: 150px;
  height: 150px;
}
.grid svg path {
  color: white;
}

.top .grid {
  border-width: 5px;
  border-top: 0;
}
.grid-wrapper .grid:last-child {
  border-right: 0;
}

.middle .grid {
  border-width: 5px;
  border-top: 0;
}

.bottom .grid {
  border-width: 5px;
  border-top: 0;
  border-bottom: 0;
}

.player-text-wrapper {
  padding-top: 60px;
  margin: auto;
  width: 70%;
  display: flex;
  justify-content: space-between;
  font-size: 26px;
  line-height: 26px;
}
.score-text-wrapper {
  margin: auto;
  width: 70%;
  display: flex;
  justify-content: space-between;
  font-size: 56px;
  line-height: 60px;
}
</style>
