<template>
  <div id="app">
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
