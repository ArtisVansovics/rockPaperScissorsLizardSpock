<template>
  <button class="button" :disabled="isDisabled" @click="clickHandler">
    <img class="img" :src="imgUrl" :alt="imgName" />
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'RoundButton',
  props: {
    isDisabled: {
      type: Boolean,
      default: false,
    },
    imgUrl: {
      type: String,
      default: '',
    },
    imgName: {
      type: String,
      default: '',
    },
  },
  emits: ['on-click'],
  methods: {
    clickHandler() {
      this.$emit('on-click');
    },
  },
});
</script>

<style scoped lang="scss" src="./RoundButton.scss" />
