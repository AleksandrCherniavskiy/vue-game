<template>
  <div class="container">
    <div class="controls">
      <div>{{stepCounter}} moves</div>
      <button @click="start">Start</button>
      <button>Reset</button>

    </div>
    <div class="board">
      <Card v-for="card in cardsArray"
            :card="card"
            @select-card="onCardSelect"
            :class="{disabled: disableCardClick}"
            :key="card.id">
        {{ card }}
      </Card>
    </div>
  </div>
<!--  <img alt="Vue logo" src="./assets/logo.png">-->
<!--  <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>-->
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import HelloWorld from './components/HelloWorld.vue';
import Card from './components/Card.vue';
import {CardItem} from "@/models/cardItem";

@Options({
  components: {
    HelloWorld,
    Card
  },
})
export default class App extends Vue {
  cardsArray: CardItem[] = [
    {id: 1, value: 1, isGuessed: false, isPreviewed: false},
    {id: 2, value: 2, isGuessed: false, isPreviewed: false},
    {id: 3, value: 3, isGuessed: false, isPreviewed: false},
    {id: 4, value: 4, isGuessed: false, isPreviewed: false},
    {id: 5, value: 5, isGuessed: false, isPreviewed: false},
    {id: 6, value: 6, isGuessed: false, isPreviewed: false},
    {id: 7, value: 8, isGuessed: false, isPreviewed: false},
    {id: 8, value: 10, isGuessed: false, isPreviewed: false},
    {id: 9, value: 9, isGuessed: false, isPreviewed: false},
    {id: 10, value: 7, isGuessed: false, isPreviewed: false},
    {id: 11, value: 10, isGuessed: false, isPreviewed: false},
    {id: 12, value: 8, isGuessed: false, isPreviewed: false},
    {id: 13, value: 9, isGuessed: false, isPreviewed: false},
    {id: 14, value: 7, isGuessed: false, isPreviewed: false},
    {id: 15, value: 6, isGuessed: false, isPreviewed: false},
    {id: 16, value: 5, isGuessed: false, isPreviewed: false},
    {id: 17, value: 4, isGuessed: false, isPreviewed: false},
    {id: 18, value: 3, isGuessed: false, isPreviewed: false},
    {id: 19, value: 2, isGuessed: false, isPreviewed: false},
    {id: 20, value: 1, isGuessed: false, isPreviewed: false},
  ];
  private disableCardClick = true;
  private previewIndexArray: number[] = [];
  // private previewCounter = 0;
  private stepCounter = 0;

  start() {
    this.disableCardClick = false;
  }

  onCardSelect(id: number) {
    const cardIndex = this.cardsArray.findIndex(item => item.id === id);
    this.previewIndexArray.push(cardIndex);

    if (!this.cardsArray[cardIndex].isGuessed && !this.cardsArray[cardIndex].isPreviewed) {
      this.cardsArray[cardIndex].isPreviewed = true;
      // this.previewCounter++;
      if (this.previewIndexArray.length === 2) {
        this.disableCardClick = true;
        this.stepCounter++;
        const card1: CardItem = this.cardsArray[this.previewIndexArray[0]];
        const card2: CardItem = this.cardsArray[this.previewIndexArray[1]];
        if (card1.value === card2.value) {
          setTimeout(() => {
            card1.isGuessed = card2.isGuessed = true;
            this.disableCardClick = false;
          }, 2000);
        } else {
          setTimeout(() => {
            card1.isPreviewed = card2.isPreviewed = false;
            this.disableCardClick = false;
          }, 2000);
        }
        this.previewIndexArray.length = 0;
      }
    }


  }
}
</script>

<style lang="scss">
  body {
    background: #ecf0f3;
  }

  .container {
    width: 1100px;
    margin: 0 auto;
  }

  .controls {
    display: flex;
    justify-content: space-between;
  }

  .board {
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-content: space-around;
    flex-wrap: wrap;
    padding: 30px 0;
  }

  .disabled {
    pointer-events: none;
  }
</style>
