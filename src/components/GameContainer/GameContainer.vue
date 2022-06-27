<template>
  <div class="container">
    <!-- Header row -->
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
      <h2 v-if="winner" class="container__win">
        {{ winMessage }}
      </h2>
    </div>
    <!-- Main row -->
    <div class="container__grid">
      <!-- P1 button column -->
      <div class="container__column">
        <div class="button-grid">
          <RoundButton
            v-for="{ id, imgUrl, imgName } in buttonImages"
            :key="id"
            :img-name="imgName"
            :img-url="imgUrl"
            class="button-grid__item"
            @on-click="selectHandlerPlayerOne(imgName)" />
        </div>
      </div>
      <!-- P1 selection column -->
      <div class="container__column">
        <img
          v-if="playerOneSelection"
          :src="playerOneImageUrl"
          :alt="playerOneSelection"
          :class="['img', { loser: winner === 'playerTwo' }]" />
      </div>
      <!-- P2 selection column -->
      <div class="container__column">
        <img
          v-if="playerTwoSelection"
          :src="playerTwoImageUrl"
          :alt="playerTwoSelection"
          :class="['img', { loser: winner === 'playerOne' }]" />
      </div>
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
      <h3 v-if="winner" class="container__description">
        {{ matchDescription }}
      </h3>
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
import type PlayerOption from '@/models/PlayerOptionModel';

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
    playerOneSelection: '' as PlayerOption,
    playerTwoSelection: '' as PlayerOption,
    winner: '',
    buttonImages: buttonImages as ButtonImage[],
  }),
  computed: {
    isResetDisabled(): boolean {
      return !this.playerOneScore && !this.playerTwoScore && !this.tieScore;
    },
    playerOneImageUrl(): string | undefined {
      return buttonImages.find(
        (button) => button.imgName === this.playerOneSelection
      )?.imgUrl;
    },
    playerTwoImageUrl(): string | undefined {
      return buttonImages.find(
        (button) => button.imgName === this.playerTwoSelection
      )?.imgUrl;
    },
    winMessage(): string {
      if (this.winner === 'none') {
        return "It's a tie!";
      } else if (this.winner === 'playerOne') {
        return 'Player one scores!';
      } else if (this.winner === 'playerTwo') {
        return 'Player two scores!';
      } else return '';
    },
    matchDescription(): string {
      if (this.playerOneSelection === this.playerTwoSelection) {
        return 'Nothing happens...';
      } else if (
        [this.playerOneSelection, this.playerTwoSelection].includes('scissors')
      ) {
        if (
          [this.playerOneSelection, this.playerTwoSelection].includes('paper')
        )
          return 'Scissors cut Paper';
        else if (
          [this.playerOneSelection, this.playerTwoSelection].includes('spock')
        )
          return 'Spock smashes Scissors';
        else if (
          [this.playerOneSelection, this.playerTwoSelection].includes('lizard')
        )
          return 'Scissors decapitate Lizard';
        else if (
          [this.playerOneSelection, this.playerTwoSelection].includes('rock')
        )
          return 'Rock crushes Scissors';
      } else if (
        [this.playerOneSelection, this.playerTwoSelection].includes('paper')
      ) {
        if ([this.playerOneSelection, this.playerTwoSelection].includes('rock'))
          return 'Paper covers Rock';
        else if (
          [this.playerOneSelection, this.playerTwoSelection].includes('lizard')
        )
          return 'Lizard eats Paper';
        else if (
          [this.playerOneSelection, this.playerTwoSelection].includes('spock')
        )
          return 'Paper disproves Spock';
      } else if (
        [this.playerOneSelection, this.playerTwoSelection].includes('rock')
      ) {
        if (
          [this.playerOneSelection, this.playerTwoSelection].includes('lizard')
        )
          return 'Rock crushes Lizard';
        else if (
          [this.playerOneSelection, this.playerTwoSelection].includes('spock')
        )
          return 'Spock vaporizes Rock';
      } else if (
        [this.playerOneSelection, this.playerTwoSelection].includes('lizard')
      ) {
        if (
          [this.playerOneSelection, this.playerTwoSelection].includes('spock')
        )
          return 'Lizard poisons Spock';
      }
      return '';
    },
  },
  methods: {
    selectHandlerPlayerOne(selection: PlayerOption): void {
      this.playerOneSelection = selection;
      this.playerTwoSelection = this.cpuSelectionRandomizer();

      this.determineWinningPlayer();
    },
    cpuSelectionRandomizer(): PlayerOption {
      return buttonImages[~~(Math.random() * buttonImages.length)].imgName;
    },
    determineWinningPlayer(): void {
      if (this.playerOneSelection === this.playerTwoSelection) {
        this.tieScore += 1;

        this.winner = 'none';
      } else if (this.playerOneSelection === 'scissors') {
        if (
          this.playerTwoSelection === 'paper' ||
          this.playerTwoSelection === 'lizard'
        ) {
          this.playerOneScore += 1;

          this.winner = 'playerOne';
        } else if (
          this.playerTwoSelection === 'rock' ||
          this.playerTwoSelection === 'spock'
        ) {
          this.playerTwoScore += 1;

          this.winner = 'playerTwo';
        }
      } else if (this.playerOneSelection === 'paper') {
        if (
          this.playerTwoSelection === 'rock' ||
          this.playerTwoSelection === 'spock'
        ) {
          this.playerOneScore += 1;

          this.winner = 'playerOne';
        } else if (
          this.playerTwoSelection === 'lizard' ||
          this.playerTwoSelection === 'scissors'
        ) {
          this.playerTwoScore += 1;

          this.winner = 'playerTwo';
        }
      } else if (this.playerOneSelection === 'rock') {
        if (
          this.playerTwoSelection === 'lizard' ||
          this.playerTwoSelection === 'scissors'
        ) {
          this.playerOneScore += 1;

          this.winner = 'playerOne';
        } else if (
          this.playerTwoSelection === 'spock' ||
          this.playerTwoSelection === 'paper'
        ) {
          this.playerTwoScore += 1;

          this.winner = 'playerTwo';
        }
      } else if (this.playerOneSelection === 'lizard') {
        if (
          this.playerTwoSelection === 'spock' ||
          this.playerTwoSelection === 'paper'
        ) {
          this.playerOneScore += 1;

          this.winner = 'playerOne';
        } else if (
          this.playerTwoSelection === 'scissors' ||
          this.playerTwoSelection === 'rock'
        ) {
          this.playerTwoScore += 1;

          this.winner = 'playerTwo';
        }
      } else if (this.playerOneSelection === 'spock') {
        if (
          this.playerTwoSelection === 'scissors' ||
          this.playerTwoSelection === 'rock'
        ) {
          this.playerOneScore += 1;

          this.winner = 'playerOne';
        } else if (
          this.playerTwoSelection === 'paper' ||
          this.playerTwoSelection === 'lizard'
        ) {
          this.playerTwoScore += 1;

          this.winner = 'playerTwo';
        }
      }
    },
    resetState(): void {
      this.playerOneScore = 0;
      this.playerTwoScore = 0;
      this.tieScore = 0;
      this.playerOneSelection = '';
      this.playerTwoSelection = '';
      this.winner = '';
    },
    // resetState(): void {
    //   Object.assign(this.$data, this.$options.data?.call(this));
    // },
  },
});
</script>

<style scoped lang="scss" src="./GameContainer.scss" />
