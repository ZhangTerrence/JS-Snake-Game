<template>
  <div id="app">
    <span id="title" class="title">Snake Game</span>
    <h1 id="subtitle" class="subtitle">Press Any Key to Start</h1>
    <h2 id="score">{{score}}</h2>
    <Game :gameRunning="running" :direction="direction"/>
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
  public direction: string = ""
  private running: boolean = false
  private score: number = 0

  public addScore(): number{
    return this.score += 1
  }

  mounted(){
    window.addEventListener("keydown", (event) => {
      const title = document.getElementById("title")!
      const subtitle = document.getElementById("subtitle")!
      title.style.opacity = "0%"
      subtitle.style.opacity = "0%"
      title.style.display = "none"
      subtitle.style.display = "none"
      this.running = true

      // Sets direction for up
      if (this.running && event.keyCode === 38 && this.direction !== "DOWN"){
        this.direction = "UP"
      }
      // Sets direction for down
      if (this.running && event.keyCode === 40 && this.direction !== "UP"){
        this.direction = "DOWN"
      }
      // Sets direction for left
      if (this.running && event.keyCode === 37 && this.direction !== "RIGHT"){
        this.direction = "LEFT"
      }
      // Sets direction for right
      if (this.running && event.keyCode === 39 && this.direction !== "LEFT"){
        this.direction = "RIGHT"
      }
    })
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

#score{
  position: absolute;
  font-size: 2.5rem;
  top: 4rem;
}

.title{
  font-size: 10rem;
  transition: all 1s;
  z-index: 10;
}
.subtitle{
  font-size: 2.5rem;
  transition: all .3s;
  z-index: 10;
}
</style>
