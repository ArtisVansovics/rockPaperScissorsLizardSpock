<template>
  <div class="container">
    <!-- Score row -->
    <div class="container__row">
      <div class="container__box">
        <h2 class="container__score">P1 (user)</h2>
        <h2 class="container__score">
          {{ playerOneScore }}
        </h2>
      </div>
      <div class="container__box">
        <h2 class="container__score">Tie</h2>
        <h2 class="container__score">
          {{ tieScore }}
        </h2>
      </div>
      <div class="container__box">
        <h2 class="container__score">P2 (CPU)</h2>
        <h2 class="container__score">
          {{ playerTwoScore }}
        </h2>
      </div>
    </div>
    <!-- Win message row -->
    <div class="container__row">
      <h3>{{ winMessage }}</h3>
    </div>
    <!-- Main row -->
    <div class="container__grid">
      <!-- P1 button column -->
      <div class="container__column">
        <div class="button-grid">
          <RoundButton
            v-for="{ id, imgUrl, imgName } in buttonImages"
            :key="id"
            class="button-grid__item"
            :img-url="imgUrl"
            :img-name="imgName" />
        </div>
      </div>
      <!-- P1 selection column -->
      <div class="container__column"></div>
      <!-- Winner column -->
      <div class="container__column"></div>
      <!-- P2 selection column -->
      <div class="container__column"></div>
      <!-- P2 button column -->
      <div class="container__column">
        <div class="button-grid">
          <RoundButton
            v-for="{ id, imgUrl, imgName } in buttonImages"
            :key="id"
            class="button-grid__item"
            :img-url="imgUrl"
            :img-name="imgName"
            is-disabled />
        </div>
      </div>
    </div>
    <!-- Match description row -->
    <div class="container__row">
      <h3>{{ matchDescription }}</h3>
    </div>
    <!-- Footer row -->
    <div class="container__row">
      <PrimaryButton :is-disabled="isResetDisabled" @on-click="resetState">
        RESET
      </PrimaryButton>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import type ButtonImage from '@/models/ButtonImageModel';
import buttonImages from '@/data/buttonImages';
import PrimaryButton from '@/components/PrimaryButton/PrimaryButton.vue';
import RoundButton from '@/components/RoundButton/RoundButton.vue';

export default defineComponent({
  name: 'GameContainer',
  components: { RoundButton, PrimaryButton },
  data: () => ({
    playerOneScore: 0,
    playerTwoScore: 0,
    tieScore: 0,
    winMessage: '',
    matchDescription: '',
    buttonImages: buttonImages as ButtonImage[],
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
