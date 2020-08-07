<template>
  <div class="monsterslayer">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">YOU</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            :style="{width: `${playerHP}%`,maxWidth:'130%',backgroundColor: 'green', margin: 0, color: 'white'}"
          >{{playerHP}}</div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">MONSTER</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            :style="{width: `${monsterHP}%`,backgroundColor: 'green', margin: 0, color: 'white'}"
          >{{monsterHP}}</div>
        </div>
      </div>
    </section>
    <section v-if="gameStarted = !gameStarted" class="row controls">
      <div class="small-12 columns">
        <button @click="startNewGame" id="start-game">START NEW GAME</button>
      </div>
    </section>
    <section v-if="gameStarted = !gameStarted" class="row controls">
      <div class="small-12 columns">
        <button @click="attack(10)" id="attack">ATTACK</button>
        <button @click="attack(20)" id="special-attack">SPECIAL ATTACK</button>
        <button @click="heal()" id="heal">HEAL</button>
        <button @click="gameStarted = !gameStarted" id="give-up">GIVE UP</button>
      </div>
    </section>
    <section v-if="attackFeed.length" class="row log">
      <div class="small-12 columns">
        <ul>
          <li
            v-for="(damage, idx) of attackFeed"
            :key="idx"
            :class="{'player-turn': idx % 2 !== 0, 'monster-turn': idx % 2 === 0 }"
          >{{damage}}</li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "MonsterSlayer",
  data() {
    return {
      gameStarted: false,
      playerHP: 100,
      monsterHP: 100,
      attackFeed: [],
    };
  },
  methods: {
    randomDamage(dmg) {
      return Math.floor(Math.random() * Math.floor(dmg));
    },
    startNewGame() {
      this.playerHP = 100;
      this.monsterHP = 100;
      this.attackFeed = [];
      return (this.gameStarted = true);
    },
    attack(damage) {
      var monsterDamage = this.randomDamage(damage);
      var playerDamage = this.randomDamage(damage);
      this.monsterHP -= playerDamage;
      this.attackFeed.unshift(`PLAYER HITS MONSTER FOR ${playerDamage}`);
      this.playerHP -= monsterDamage;
      this.attackFeed.unshift(`MONSTER HITS PLAYER FOR ${monsterDamage}`);

      console.log(this.attackFeed);
    },
    heal() {
      var monsterDamage = this.randomDamage(10);
      this.playerHP += 10;
      this.playerHP -= monsterDamage;
      this.attackFeed.unshift(`PLAYER HEALS HIMSELF FOR 10`);
      this.attackFeed.unshift(`MONSTER HITS PLAYER FOR ${monsterDamage}`);
    },
  },
  watch: {
    playerHP(hp) {
      if (hp <= 0) {
        alert("You loose, start new game?");
        this.gameStarted = false;
      }
    },
    monsterHP(hp) {
      if (hp <= 0) {
        alert("You win, start new game?");
        this.gameStarted = false;
      }
    },
  },
  computed: {
    displayGame() {
      return (gameStarted = !gameStarted);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*  */
</style>
