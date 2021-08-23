<template>
  <h1>VueJS Turn-Based Game</h1>
  <div class="healthGUI">
    <h2>Enemy</h2>
    <div class="enemyHealthBar" :style="enemyHealthStyles"></div>
  </div>
  <div class="healthGUI">
    <h2>Player</h2>
    <div class="playerHealthBar" :style="playerHealthStyles"></div>
  </div>
  <div class="endGameGUI" v-if="gameWinner">
    <h3>{{ gameWinner }} win the game !</h3>
    <button>Restart Game</button>
  </div>
  <div class="gameControlsGUI" v-if="gameWinner === null">
    <button @click="attackEnemy">Attack</button>
    <button @click="specialAttackEnemy" :disabled="!specialAttackAvailability">
      Special Attack
    </button>
    <button @click="healPlayer" :disabled="!healAvailability">Heal</button>
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
      this.attackPlayer();
    },
    attackPlayer() {
      const attackPoint = Math.floor(Math.random() * (15 - 5) + 5);
      this.playerHealth -= attackPoint;
    },
    specialAttackEnemy() {
      const attackPoint = Math.floor(Math.random() * (25 - 15) + 15);
      this.enemyHealth -= attackPoint;
      this.specialAttackCountdown = 3;
      this.healCountdown--;
      this.attackPlayer();
    },
    healPlayer() {
      const healPoint = Math.floor(Math.random() * (20 - 10) + 10);
      this.playerHealth += healPoint;
      this.specialAttackCountdown--;
      this.healCountdown = 3;
      this.attackPlayer();
    },
  },
};
</script>

<style>
.healthGUI {
  width: 25%;
  position: relative;
}
.enemyHealthBar,
.playerHealthBar {
  height: 50px;
  border-radius: 10px;
}
.enemyHealthBar {
  background-color: red;
}
.playerHealthBar {
  background-color: green;
}
.enemyHealthBar::before,
.playerHealthBar::before {
  content: "";
  position: absolute;
  z-index: -1;
  width: 100%;
  border-radius: 10px;
  height: 50px;
  background-color: grey;
}
.gameControlsGUI {
  padding: 20px;
}
.gameControlsGUI button,
.endGameGUI button {
  font-size: 16pt;
  padding: 15px;
  border-radius: 10px;
}
</style>
