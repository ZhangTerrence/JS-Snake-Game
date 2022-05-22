<template>
  <div id="game" ref="board">
    <div v-for="(i, index) in squares" :key="index">
      <Tile :coords="i.coordinate"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Tile from "../components/Tile.vue"

@Component({
  components:{
    Tile
  }
})
export default class Game extends Vue {
  private squares: Array<object> = []

  // Sets coordinates of individual tile for later use
  public setCoordinate(): any{
    // Iterate for x
    for (let x = 1; x < 21; x++){
      // Iterate for y
      for (let y = 1; y < 21; y++){
        this.squares.push({coordinate: (`${x}, ${y}`)})
      }
    }
  }

  mounted(){
    this.setCoordinate()
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
