<template>
  <button class="button" :disabled="isDisabled" @click="clickHandler">
    <slot></slot>
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'PrimaryButton',
  props: {
    isDisabled: Boolean,
  },
  emits: ['on-click'],
  methods: {
    clickHandler() {
      this.$emit('on-click');
    },
  },
});
</script>

<style scoped lang="scss" src="./PrimaryButton.scss" />
