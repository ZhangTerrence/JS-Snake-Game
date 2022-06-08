<template>
    <section id="endscreen">
        <h3 id="endscore">Your score is: {{endscore}}</h3>
        <button id="replay" class="btn" @click="restart">Replay</button>
    </section>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component
export default class Endscreen extends Vue {
    @Prop({required: true}) endscore!: number

    public restart(): void{
        window.location.reload()
    }
}
</script>

<style scoped>
#endscreen{
    width: 100vw;
    height: 100vh;
    display: none;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
}

#endscore{
    font-size: 5rem;
}

.btn {
    cursor: pointer;
    display: block;
    font-weight: 400;
    line-height: 45px;
    margin: 0 0 2em;
    position: relative;
    text-decoration: none;
    text-transform: uppercase;
    width: 30rem;
    height: 10rem;
    font-size: 4rem;
}  

#replay {
    border: 0 solid;
    box-shadow: inset 0 0 20px rgba(255, 255, 255, 0);
    outline: 1px solid;
    outline-color: #000;
    outline-offset: 0px;
    text-shadow: none;
    transition: all 1250ms cubic-bezier(0.19, 1, 0.22, 1);
} 

#replay:hover {
  border: 1px solid;
  box-shadow: inset 0 0 20px rgba(255, 255, 255, .5), 0 0 20px rgba(255, 255, 255, .2);
  outline-color: rgba(255, 255, 255, 0);
  outline-offset: 15px;
  text-shadow: 1px 1px 2px #000; 
}
</style>