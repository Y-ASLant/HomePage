<script setup lang="ts">
import { Typed } from '@duskmoon/vue3-typed-js';
import type { TypedOptions } from '@duskmoon/vue3-typed-js';

const options: TypedOptions = {
  strings: ['Hello!', "I'm ASLant."],
  loop: true,
  backSpeed: 30,
  typeSpeed: 50,
  startDelay: 120
};
</script>

<template>
  <div class="meBox-title">
    <Typed :options="options">
      <span class="typing"></span>
    </Typed>
  </div>
</template>

<style scoped>
.meBox-title {
  font-size: 3rem;
  color: #fffffff3;
  animation: floatUp 1s ease-out;
  font-family:
    'JetBrains Mono', ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono',
    'Courier New', monospace;
}

/* 确保打字内容也继承同一字体 */
.meBox-title .typing {
  font-family: inherit;
}
</style>
