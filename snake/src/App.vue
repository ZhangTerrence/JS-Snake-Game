<template>
  <div id="app">
    <span id="title" class="title">Snake Game</span>
    <h1 id="subtitle" class="subtitle">Press Any Key to Start</h1>
    <Game/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Game from './components/Game.vue';

@Component({
  components: {
    Game
  },
})
export default class App extends Vue {
  public startGame(): void{
    const title = document.getElementById("title")!
    const subtitle = document.getElementById("subtitle")!
    title.style.opacity = "0%"
    title.style.display = "none"
    subtitle.style.opacity = "0%"
    subtitle.style.display = "none"
  }
  mounted(){
    document.body.onkeyup = this.startGame
  }
}
</script>

<style>
*, body{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-size: 62.5%;
}

#app {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
}

.title{
  font-size: 10rem;
  transition: all .3s;
  z-index: 10;
}
.subtitle{
  font-size: 2.5rem;
  transition: all .3s;
  z-index: 10;
}
</style>
