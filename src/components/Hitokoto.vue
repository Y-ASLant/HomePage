<script setup>
import { computed, onMounted, ref } from 'vue';
import StaggerText from './StaggerText.vue';

const STAGGER_DELAY = 0.015;
const CHAR_DURATION = 0.3;
const SOURCE_GAP = 0.08;

const hitokoto = ref({
  hitokoto: '不以物喜，不以己悲。',
  from: '岳阳楼记'
});
const animationKey = ref(0);

const sourceText = computed(() => `——「${hitokoto.value.from}」`);
const bodyLength = computed(() => hitokoto.value.hitokoto.length);
const sourceStartDelay = computed(() => {
  if (bodyLength.value === 0) {
    return CHAR_DURATION + SOURCE_GAP;
  }
  return (bodyLength.value - 1) * STAGGER_DELAY + CHAR_DURATION + SOURCE_GAP;
});

async function getHitokoto() {
  try {
    const response = await fetch(
      'https://api.timelessq.com/sentence?type=a,b,c,d,e,h,i,j,k,l&minLength=5&maxLength=20'
    );
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    const result = await response.json();
    const data = result?.data;
    if (!data?.hitokoto) {
      throw new Error('Invalid response payload');
    }

    hitokoto.value = {
      hitokoto: data.hitokoto,
      from: data.from || '未知来源'
    };
    animationKey.value += 1;
  } catch (error) {
    console.error('获取一言失败:', error);
    hitokoto.value = {
      hitokoto: '不以物喜，不以己悲。',
      from: '岳阳楼记'
    };
    animationKey.value += 1;
  }
}

onMounted(() => {
  getHitokoto();
});
</script>

<template>
  <div class="meBox-hitokoto">
    <div class="bracket left">“</div>
    <StaggerText
      :animation-key="animationKey"
      :text="hitokoto.hitokoto"
      :stagger-delay="STAGGER_DELAY"
      :duration="CHAR_DURATION"
    />
    <div class="bracket right">”</div>
    <StaggerText
      :animation-key="`source-${animationKey}`"
      :text="sourceText"
      :base-delay="sourceStartDelay"
      :stagger-delay="STAGGER_DELAY"
      :duration="CHAR_DURATION"
      class="right-align source-line"
    />
  </div>
</template>

<style scoped>
.meBox-hitokoto {
  font-size: 1.2rem;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.26);
  border-right: 0 solid;
}

.right-align {
  text-align: right;
}

.source-line {
  min-height: 1.5em;
}

.bracket {
  font-size: 2rem;
  color: #fff;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.26);
}

.bracket.left {
  text-align: left;
}

.bracket.right {
  text-align: right;
}
</style>
