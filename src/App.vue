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
  <div class="gameControlsGUI">
    <button @click="attackEnemy">Attack</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerHealth: 100,
      enemyHealth: 100,
    };
  },
  computed: {
    playerHealthStyles() {
      return { width: this.playerHealth + "%" };
    },
    enemyHealthStyles() {
      return { width: this.enemyHealth + "%" };
    },
  },
  methods: {
    attackEnemy() {
      const attackPoint = Math.floor(Math.random() * (15 - 5) + 5);
      this.enemyHealth -= attackPoint;
      this.attackPlayer();
    },
    attackPlayer() {
      const attackPoint = Math.floor(Math.random() * (15 - 5) + 5);
      this.playerHealth -= attackPoint;
    },
  },
};
</script>

<style>
.healthGUI {
  width: 25%;
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
.gameControlsGUI {
  padding: 20px;
}
.gameControlsGUI button {
  font-size: 16pt;
  padding: 15px;
  border-radius: 10px;
}
</style>
