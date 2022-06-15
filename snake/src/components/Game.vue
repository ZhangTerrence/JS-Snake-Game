<template>
  <section id="game">
    <div v-for="(i, index) in squares" :key="index">
      <Tile :id="i.coordinate"/>
    </div>
  </section>
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
  private snakeSegments: Array<any> = []

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

  // Gets initial head of snake
  public startSnake(): void{
    this.snakeSegments.push(spawnInfo.snakeStart)
    this.snakify()
  }

  // Applify a tile
  public setApple(): void{
    const appleCoordinate = (`${Math.floor(Math.random() * (20 - 1) + 1)}, ${Math.floor(Math.random() * (20 - 1) + 1)}`)
    const appleTile = document.getElementById(appleCoordinate)!
     // If a snake tile is detected try again
    if (appleTile.classList.value === "game-tile snake"){
      this.setApple()
      return
    } else {
      appleTile.classList.add("apple")
    }
  }

  public snakify(): void{
    this.snakeSegments.forEach(snakeInfo => {
      const snakeTile = document.getElementById(snakeInfo)!
      snakeTile.classList.add("snake")
    })
  }
  
  public move(x: number, y: number): void{
    // Gets the head of the snake
    const snakeHead = this.snakeSegments[0]
    // Identifies the coordinates of the next tile based on the direction
    const split = snakeHead.split(",")
    const updated = (`${parseInt(split[0]) + x}, ${parseInt(split[1]) + y}`)
    const newTile = document.getElementById(updated)!
    try {
      // Snake head collides with snake or wall
      if (newTile.classList.value === "game-tile snake"){
        this.endGame()
      } else if (newTile.classList.value === "game-tile apple"){
        // Since apple is eaten the snake's new head is at the apple's coordinate
        this.snakeSegments.unshift(updated);
        // No popping because the snake gains length
        // Unapplify the tile
        newTile.classList.remove("apple");
        (this.$parent as any).addScore();
        this.snakify()
        this.setApple() 
      } else {
        // Adds new head to the array
        this.snakeSegments.unshift(updated)
        // Removes old head from the array and identifies the corresponding tile to remove class "snake"
        const oldTile = document.getElementById(this.snakeSegments.pop())!
        oldTile.classList.remove("snake")
        this.snakify()
      } 
    } catch (error) {
      this.endGame()
    }
  }
  
  // Runs game
  public run(): void{
    const direction = this.$props.direction
    // Directions 
    if (direction === "UP"){
      this.move(-1, 0)
    }
    if (direction === "DOWN"){
      this.move(1, 0)
    }
    if (direction === "LEFT"){
      this.move(0, -1)
    }
    if (direction === "RIGHT"){
      this.move(0, 1)
    }
  }

  public endGame(): void{
    clearInterval((this.onChange as any))
    document.getElementById("endscreen")!.style.display = "flex"
    document.getElementById("game")!.style.display = "none"
    document.getElementById("score")!.style.display = "none"
    document.getElementById("title")!.style.display = "none"
    document.getElementById("subtitle")!.style.display = "none"
  }

  mounted(){
    // Once game is loaded, add coordinates to each tile
    this.setCoordinate()
    // Emits the initial direction to App.vue
    this.$emit("getInitial", this.initialDirection)
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
