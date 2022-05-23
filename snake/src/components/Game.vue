<template>
  <div id="game">
    <div v-for="(i, index) in squares" :key="index">
      <Tile :id="i.coordinate"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import Tile from "../components/Tile.vue"
import spawnInfo from "../config/spawnInfo"
import directions from "../config/directions"

@Component({
  components:{
    Tile
  }
})

export default class Game extends Vue {
  @Prop({default: null}) gameRunning!: boolean
  @Prop({default: null}) direction!: string

  @Watch("gameRunning")
  onChange(){
    window.setInterval(() => {
      this.run()
    }, 750)
  }

  private squares: Array<object> = []
  private initialDirection: string = this.getInitialDirection(spawnInfo.initalMatch)
  private snakeHead: string = ""

  public getInitialDirection(num: number): string{
   return (directions as any)[num]
  }

  // Sets coordinates of individual tile for later use
  public setCoordinate(): void{
    // Iterate for x
    for (let x = 1; x < 21; x++){
      // Iterate for y
      for (let y = 1; y < 21; y++){
        this.squares.push({coordinate: (`${x}, ${y}`)})
      }
    }
  }

  // Snakify a tile
  public startSnake(): void{
    const snakeTile = document.getElementById(spawnInfo.snakeStart)!
    this.snakeHead = spawnInfo.snakeStart
    snakeTile.classList.add("snake")
  }

  // Applify a tile
  public setApple(): void{
    const appleCoordinate = (`${Math.floor(Math.random() * (20 - 1) + 1)}, ${Math.floor(Math.random() * (20 - 1) + 1)}`)
    const appleTile = document.getElementById(appleCoordinate)!
    appleTile.classList.add("apple")
  }

  public predict(x: number, y: number): any{
    const split = this.snakeHead.split(",")
    const newHead = (`${parseInt(split[0]) + x}, ${parseInt(split[1]) + y}`)
    const oldTile = document.getElementById(this.snakeHead)!
    const newTile = document.getElementById(newHead)!
    if (newTile.classList.value === "game-tile apple"){
      this.snakeHead = newHead
      newTile.classList.add("snake")
      newTile.classList.remove("apple");
      (this.$parent as any).addScore()
      this.setApple()
    } else{
      this.snakeHead = newHead
      oldTile.classList.remove("snake")
      newTile.classList.add("snake")
    }
    
    
    // If newHead goes out of bound, end game
  }
  
  // Runs game
  public run(): void{
    const direction = this.$props.direction
    
    // Directions are currently a bit messed up but still works
    if (direction === "UP"){
      this.predict(-1, 0)
    }
    if (direction === "DOWN"){
      this.predict(1, 0)
    }
    if (direction === "LEFT"){
      this.predict(0, -1)
    }
    if (direction === "RIGHT"){
      this.predict(0, 1)
    }
  }

  mounted(){
    // Once game is loaded, add coordinates to each tile
    this.setCoordinate()
  }
  updated(){
    this.startSnake()
    this.setApple()
  }


}
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#game{
  box-sizing: unset;
  position: absolute;
  width: 50rem;
  height: 50rem;
  border: solid 1rem rgba(128, 128, 128, 0.127);
}
</style>
