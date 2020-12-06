<template>
  <div class="container">
    <div class="controls">
      <div class="controls__item">{{ moves }} moves</div>
      <button class="controls__item controls__item_btn" @click="start"
              v-if="!gameStarted">Start
      </button>
      <button class="controls__item controls__item_btn" @click="reset" v-else>Reset</button>
      <Timer class="controls__item"
             :timerEvent="timerEvent"></Timer>
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
</template>

<script lang="ts">
  import {Options, Vue} from 'vue-class-component';
  import Card from './components/Card.vue';
  import {CardItem} from "@/models/cardItem";
  import Timer from '@/components/Timer.vue';

  @Options({
    components: {
      Card,
      Timer
    },
  })

  export default class App extends Vue {

    private cardsArray: CardItem[] = [
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
    private gameStarted = false;
    private moves = 0;
    private timerEvent = '';

    start() {
      this.disableCardClick = false;
      this.timerEvent = 'start';
      this.gameStarted = true;
    }

    reset() {
      this.disableCardClick = false;
      this.timerEvent = 'stop';
      this.cardsArray.forEach(item => {
        item.isGuessed = false;
        item.isPreviewed = false;
      });
      this.moves = 0;
      setTimeout(() => {
        this.timerEvent = 'start';
      }, 0);

    }

    onCardSelect(id: number) {
      const cardIndex = this.cardsArray.findIndex(item => item.id === id);
      this.previewIndexArray.push(cardIndex);

      if (!this.cardsArray[cardIndex].isGuessed && !this.cardsArray[cardIndex].isPreviewed) {
        this.cardsArray[cardIndex].isPreviewed = true;
        if (this.previewIndexArray.length === 2) {
          this.disableCardClick = true;
          setTimeout(() => {
            this.moves++;
            const card1: CardItem = this.cardsArray[this.previewIndexArray[0]];
            const card2: CardItem = this.cardsArray[this.previewIndexArray[1]];
            if (card1.value === card2.value) {
              card1.isGuessed = card2.isGuessed = true;
              this.disableCardClick = false;
              if (this.cardsArray.every(item => item.isGuessed)) {
                window.alert('Congratulations, you win!');
                this.timerEvent = 'stop';
                this.disableCardClick = true;
              }
            } else {
              card1.isPreviewed = card2.isPreviewed = false;
              this.disableCardClick = false;
            }
            this.previewIndexArray.length = 0;
          }, 1000);
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
    align-items: center;

    &__item {
      width: 100px;
      text-align: center;
      font-size: 20px;

      &_btn {
        height: 50px;
        background-color: #4CAF50;
        border: none;
        border-radius: 10px;
        color: white;
        text-decoration: none;
        display: inline-block;
        transition-duration: 0.4s;
        cursor: pointer;
      }
    }
  }

  .board {
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-content: space-around;
    flex-wrap: wrap;
    padding: 15px 0;
  }

  .disabled {
    pointer-events: none;
  }

</style>
