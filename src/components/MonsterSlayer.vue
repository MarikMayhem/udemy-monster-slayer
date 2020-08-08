<template>
  <div class="monsterslayer">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">YOU</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            :style="{width: `${playerHP}%`, backgroundColor: 'green', margin: 0, color: 'white'}"
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
    <section v-if="!gameStarted" class="row controls">
      <div class="small-12 columns">
        <button @click="startNewGame" id="start-game">START NEW GAME</button>
      </div>
    </section>
    <section v-if="gameStarted" class="row controls">
      <div class="small-12 columns">
        <button @click="attack('normal')" id="attack">ATTACK</button>
        <button @click="attack('special')" id="special-attack">SPECIAL ATTACK</button>
        <button @click="heal()" id="heal">HEAL</button>
        <button @click="stopGame()" id="give-up">GIVE UP</button>
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
    startNewGame() {
      this.playerHP = 100;
      this.monsterHP = 100;
      this.attackFeed = [];
      this.gameStarted = !this.gameStarted;
    },
    randomDamage(maxDmg, minDmg) {
      return Math.max(Math.floor(Math.random() * maxDmg) + 1, minDmg);
    },
    stopGame() {
      return (this.gameStarted = false);
    },
    monsterAttack() {
      var monsterRange = 12;
      var minMonsterRange = 5;
      var monsterDamage = this.randomDamage(monsterRange, minMonsterRange);
      this.playerHP -= monsterDamage;
      this.attackFeed.unshift(`MONSTER HITS PLAYER FOR ${monsterDamage}`);
    },
    playerAttack(kind) {
      //NORMAL : SPECIAL
      var playerRange = kind === "normal" ? 10 : 20;
      var minPlayerRange = kind === "normal" ? 3 : 10;
      var playerDamage = this.randomDamage(playerRange, minPlayerRange);
      var message =
        kind === "normal"
          ? `PLAYER HITS MONSTER FOR ${playerDamage}`
          : `PLAYER HITS MONSTER HARD FOR ${playerDamage}`;
      this.monsterHP -= playerDamage;
      this.attackFeed.unshift(message);
    },
    attack(damageKind) {
      this.playerAttack(damageKind);
      this.monsterAttack();

      this.playerHP <= 0 ? alert("You loose, start new game?") : "";
      this.monsterHP <= 0 ? alert("You Win, start new game?") : "";
    },
    heal() {
      this.playerHP <= 90 ? (this.playerHP += 10) : (this.playerHP = 100);
      this.attackFeed.unshift(`PLAYER HEALS HIMSELF FOR 10`);

      this.monsterAttack();
    },
  },
};
</script>
