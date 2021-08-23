<template>
  <h1>VueJS Turn-Based Game</h1>
  <div class="healthGUI">
    <h2>Enemy</h2>
    <div class="healthContainer">
      <div class="enemyHealthBar" :style="enemyHealthStyles"></div>
    </div>
  </div>
  <div class="healthGUI">
    <h2>Player</h2>
    <div class="healthContainer">
      <div class="playerHealthBar" :style="playerHealthStyles"></div>
    </div>
  </div>
  <div class="endGameGUI" v-if="gameWinner">
    <h3>{{ gameWinner }} win the game !</h3>
    <button @click="restartGame">
      <i class="fa fa-refresh" aria-hidden="true"></i> Restart Game
    </button>
  </div>
  <div class="gameControlsGUI" v-if="gameWinner === null">
    <button @click="attackEnemy" id="attackButton">
      <i class="fa fa-hand-rock-o" aria-hidden="true"></i> Attack
    </button>
    <button
      @click="specialAttackEnemy"
      :disabled="!specialAttackAvailability"
      id="specialButton"
    >
      <i class="fa fa-hand-lizard-o" aria-hidden="true"></i> Special Attack
    </button>
    <button @click="healPlayer" :disabled="!healAvailability" id="healButton">
      <i class="fa fa-heart" aria-hidden="true"></i> Heal
    </button>
  </div>
  <div class="battleLogGUI">
    <h3>----- Battle Log -----</h3>
    <ul class="battleLogBox">
      <li v-for="log in battleLog" :key="log">{{ log }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerHealth: 100,
      enemyHealth: 100,
      gameWinner: null,
      specialAttackCountdown: 0,
      healCountdown: 0,
      battleLog: [],
    };
  },
  watch: {
    playerHealth(value) {
      if (value <= 0) {
        this.gameWinner = "Enemy";
      }
    },
    enemyHealth(value) {
      if (value <= 0) {
        this.gameWinner = "Player";
      }
    },
  },
  computed: {
    playerHealthStyles() {
      if (this.playerHealth < 0) {
        return { width: "0%" };
      }
      return { width: this.playerHealth + "%" };
    },
    enemyHealthStyles() {
      if (this.enemyHealth < 0) {
        return { width: "0%" };
      }
      return { width: this.enemyHealth + "%" };
    },
    specialAttackAvailability() {
      if (this.specialAttackCountdown <= 0) {
        return true;
      }
      return false;
    },
    healAvailability() {
      if (this.healCountdown <= 0) {
        return true;
      }
      return false;
    },
  },
  methods: {
    attackEnemy() {
      const attackPoint = Math.floor(Math.random() * (15 - 5) + 5);
      this.enemyHealth -= attackPoint;
      this.specialAttackCountdown--;
      this.healCountdown--;
      this.addLog("Player", "attack", attackPoint);
      this.attackPlayer();
    },
    attackPlayer() {
      const attackPoint = Math.floor(Math.random() * (15 - 5) + 5);
      this.playerHealth -= attackPoint;
      this.addLog("Enemy", "attack", attackPoint);
    },
    specialAttackEnemy() {
      const attackPoint = Math.floor(Math.random() * (25 - 15) + 15);
      this.enemyHealth -= attackPoint;
      this.specialAttackCountdown = 3;
      this.healCountdown--;
      this.addLog("Player", "attack", attackPoint);
      this.attackPlayer();
    },
    healPlayer() {
      const healPoint = Math.floor(Math.random() * (20 - 10) + 10);
      this.playerHealth += healPoint;
      this.specialAttackCountdown--;
      this.healCountdown = 3;
      this.addLog("Player", "heal", healPoint);
      this.attackPlayer();
    },
    restartGame() {
      this.playerHealth = 100;
      this.enemyHealth = 100;
      this.gameWinner = null;
      this.specialAttackCountdown = 0;
      this.healCountdown = 0;
      this.battleLog = [];
    },
    addLog(who, what, points) {
      this.battleLog.unshift(`${who} ${what} for ${points} points.`);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Mono&display=swap");
body,
button {
  font-family: "Roboto Mono";
}
body {
  padding: 0;
  margin: 0;
}
#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.healthGUI {
  width: 40%;
  display: flex;
  flex-direction: row;
}
.healthGUI h2 {
  flex: 1;
}
.healthContainer {
  width: 100%;
  height: 50px;
  border-radius: 10px;
  overflow: hidden;
  background-color: gainsboro;
  border: 1px solid black;
  padding: 0;
  flex: 3;
}
.enemyHealthBar,
.playerHealthBar {
  height: 100%;
}
.enemyHealthBar {
  background-color: red;
}
.playerHealthBar {
  background-color: green;
}
.gameControlsGUI {
  display: flex;
  width: 40%;
  flex-direction: row;
  justify-content: center;
  gap: 5%;
  padding: 20px;
}
#attackButton {
  background-color: salmon;
}
#specialButton {
  background-color: yellow;
}
#healButton {
  background-color: greenyellow;
}
.gameControlsGUI button,
.endGameGUI button {
  font-size: 16pt;
  padding: 15px;
  border-radius: 10px;
}
.battleLogGUI {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  height: 40%;
  width: 40%;
}
.battleLogBox {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  border: 1px solid black;
  background-color: gainsboro;
  overflow: auto;
}
</style>
