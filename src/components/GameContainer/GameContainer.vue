<template>
  <div class="container">
    <div class="container__row">
      <div class="container__box">
        <h2>P1 (user)</h2>
        <h2>{{ playerOneScore }}</h2>
      </div>
      <div class="container__box">
        <h2>Tie</h2>
        <h2>{{ tieScore }}</h2>
      </div>
      <div class="container__box">
        <h2>P2 (CPU)</h2>
        <h2>{{ playerTwoScore }}</h2>
      </div>
    </div>
    <div class="container__row">
      <h3>{{ winMessage }}</h3>
    </div>
    <div class="container__row">placeholder</div>
    <div class="container__row">
      <h3>{{ matchDescription }}</h3>
    </div>
    <div class="container__row">
      <h3>{{ matchDescription }}</h3>
    </div>
    <div class="container__row">
      <PrimaryButton :is-disabled="isResetDisabled" @on-click="resetState">
        RESET
      </PrimaryButton>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import PrimaryButton from '@/components/PrimaryButton/PrimaryButton.vue';

export default defineComponent({
  name: 'GameContainer',
  components: { PrimaryButton },
  data: () => ({
    playerOneScore: 0,
    playerTwoScore: 0,
    tieScore: 0,
    winMessage: '',
    matchDescription: '',
  }),
  computed: {
    isResetDisabled(): boolean {
      return !this.playerOneScore && !this.playerTwoScore && !this.tieScore;
    },
  },
  methods: {
    resetState(): void {
      this.playerOneScore = 0;
      this.playerTwoScore = 0;
      this.tieScore = 0;
      this.winMessage = '';
      this.matchDescription = '';
    },
  },
});
</script>

<style scoped lang="scss" src="./GameContainer.scss" />
