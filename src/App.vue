<template>
  <h1>VueJS Turn-Based Game</h1>
  <div class="healthGUI">
    <h2>Enemy</h2>
    <div class="enemyHealthBar"></div>
  </div>
  <div class="healthGUI">
    <h2>Player</h2>
    <div class="playerHealthBar"></div>
  </div>
  <div class="gameControlsGUI">
    <button>Attack</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nama: "wawan",
    };
  },
};
</script>

<style>
.healthGUI {
  width: 25%;
}
.enemyHealthBar,
.playerHealthBar {
  width: 100%;
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
