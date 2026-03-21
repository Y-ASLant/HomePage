<script setup>
import { onMounted, ref } from 'vue';

const hitokoto = ref({
  hitokoto: '不以物喜，不以己悲。',
  from: '岳阳楼记'
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
  } catch (error) {
    console.error('获取一言失败:', error);
    hitokoto.value = {
      hitokoto: '不以物喜，不以己悲。',
      from: '岳阳楼记'
    };
  }
}

onMounted(() => {
  getHitokoto();
});
</script>

<template>
  <div class="meBox-hitokoto">
    <div class="bracket left">“</div>
    <p>{{ hitokoto.hitokoto }}</p>
    <div class="bracket right">”</div>
    <p class="right-align">——「{{ hitokoto.from }}」</p>
  </div>
</template>

<style scoped>
.meBox-hitokoto {
  font-size: 1.2rem;
  text-shadow: #00000043;
  border-right: 0 solid;
}

.right-align {
  text-align: right;
}

.bracket {
  font-size: 2rem;
  color: #fff;
  text-shadow: #00000043;
}

.bracket.left {
  text-align: left;
}

.bracket.right {
  text-align: right;
}
</style>
