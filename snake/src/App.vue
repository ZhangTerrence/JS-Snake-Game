<template>
  <div id="app">
    <span id="title" class="title">Snake Game</span>
    <h1 id="subtitle" class="subtitle">Press Any Key to Start</h1>
    <h2 id="score">{{score}}</h2>
    <Game :gameRunning="running" :direction="direction" @getInitial="setInitial"/>
    <Endscreen :endscore="score"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Game from './components/Game.vue';
import Endscreen from './components/Endscreen.vue';

@Component({
  components: {
    Game,
    Endscreen
  },
})
export default class App extends Vue {
  public direction: string = ""
  private running: boolean = false
  private score: number = 0

  public addScore(): number{
    return this.score += 1
  }

  public setInitial(value: string): void{
    this.direction = value
  }

  public listenStart(event: KeyboardEvent): any{
    const title = document.getElementById("title")!
    const subtitle = document.getElementById("subtitle")!
    title.style.opacity = "0%"
    subtitle.style.opacity = "0%"
    this.running = true

    // Sets direction for up
    if (this.running && event.key === "ArrowUp" && this.direction !== "DOWN"){
      this.direction = "UP"
    }
    // Sets direction for down
    if (this.running && event.key === "ArrowDown" && this.direction !== "UP"){
      this.direction = "DOWN"
    }
    // Sets direction for left
    if (this.running && event.key === "ArrowLeft" && this.direction !== "RIGHT"){
      this.direction = "LEFT"
    }
    // Sets direction for right
    if (this.running && event.key === "ArrowRight" && this.direction !== "LEFT"){
      this.direction = "RIGHT"
    }
  }

  mounted(){
    window.addEventListener("keydown", (event) => {
      this.listenStart(event)
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
  transition: all .5s;
  z-index: 10;
}
.subtitle{
  font-size: 2.5rem;
  transition: all .3s;
  z-index: 10;
}
</style>
