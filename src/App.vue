<template>
  <div id="app">
    <h1 class="app__title">ROCK | PAPER | SCISSORS | LIZARD | SPOCK</h1>
    <GameContainer />
  </div>
</template>

<script lang="ts">
import GameContainer from '@/components/GameContainer/GameContainer.vue';

export default {
  components: { GameContainer },
};
</script>

<style scoped lang="scss" src="./styles/App.scss" />
