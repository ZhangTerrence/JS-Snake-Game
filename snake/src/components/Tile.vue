<template>
  <div class="game-tile" :data-coords="coords"></div>
</template>

<script>
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class Tile extends Vue {
  @Prop() coords
}

</script>

<style>
.game-tile{
  width: 2.5rem;
  height: 2.5rem;
  background: whitesmoke;
  float: left;
  border-radius: .025rem;
  border: solid .5px rgb(170, 170, 170);
}

.game-tile.snake{
  background: greenyellow;
  border: 1px solid #000;
}

.game-tile.apple{
  background: red;
  border: 1px solid #000;
}
</style>