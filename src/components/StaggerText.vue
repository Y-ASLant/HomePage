<script setup>
import { computed } from 'vue';

const props = defineProps({
  text: {
    type: String,
    default: ''
  },
  animationKey: {
    type: [String, Number],
    default: 0
  },
  tag: {
    type: String,
    default: 'p'
  },
  baseDelay: {
    type: Number,
    default: 0
  },
  staggerDelay: {
    type: Number,
    default: 0.015
  },
  duration: {
    type: Number,
    default: 0.3
  }
});

const chars = computed(() => props.text.split(''));
</script>

<template>
  <component :is="tag" :key="animationKey" class="stagger-line">
    <span
      v-for="(char, index) in chars"
      :key="`${animationKey}-${index}`"
      class="stagger-char"
      :style="{
        animationDelay: `${baseDelay + index * staggerDelay}s`,
        animationDuration: `${duration}s`
      }"
    >
      {{ char === ' ' ? '\u00A0' : char }}
    </span>
  </component>
</template>

<style scoped>
.stagger-line {
  white-space: pre-wrap;
}

.stagger-char {
  display: inline-block;
  opacity: 0;
  filter: blur(10px);
  animation-name: blurReveal;
  animation-timing-function: ease;
  animation-fill-mode: forwards;
}

@keyframes blurReveal {
  to {
    opacity: 1;
    filter: blur(0);
  }
}
</style>
