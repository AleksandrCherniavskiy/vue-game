<template>
  <div class="card"
       @click="selectCard(card.id)"
       :class="{ open: card.isPreviewed, guess: card.isGuessed}">
    {{card.value}}
  </div>
</template>

<script lang="ts">
  import {Options, Vue} from 'vue-class-component';

  @Options({
    props: {
      card: {
        type: Object,
        default: function () {
          return {value: 1}
        }
      }
    },
    emits: ['select-card']
  })

  export default class Card extends Vue {

    selectCard(id: number) {
      this.$emit('select-card', id);
    }

  }
</script>

<style scoped>
  .card {
    width: 200px;
    height: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #6467c5;
    margin: 10px 10px;
    border-radius: 32px;
    cursor: pointer;
    font-family: "Montserrat", sans-serif;
    font-size: 36px;
    font-weight: 600;
    box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8), 6px 6px 10px rgba(0, 0, 0, 0.2);
    color: #6467c5;
  }

  .open {
    background: transparent;
  }

  .guess {
    color: lightgray;
  }
</style>
