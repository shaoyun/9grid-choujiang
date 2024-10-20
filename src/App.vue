<script setup lang="ts">
import { ref } from 'vue'
import LotteryGrid from './components/LotteryGrid.vue'

const userName = ref('')
const isDrawing = ref(false)
const winners = ref<{ name: string; prize: string }[]>([])

const startDraw = () => {
  if (userName.value) {
    isDrawing.value = true
  } else {
    alert('请输入用户名')
  }
}

const stopDraw = () => {
  isDrawing.value = false
}

const handlePrizeSelected = (prize: string) => {
  if (prize !== '谢谢参与') {
    winners.value.push({ name: userName.value, prize })
    alert(`恭喜 ${userName.value} 获得 ${prize}！`)
  } else {
    alert('很遗憾，您没有中奖。')
  }
  userName.value = ''
}
</script>

<template>
  <div class="lottery-app">
    <h1>抽奖乐翻天</h1>
    <div class="user-input">
      <input v-model="userName" placeholder="请输入用户名" :disabled="isDrawing" />
    </div>
    <div class="lottery-container">
      <div class="lottery-rules">
        <h3>抽奖规则</h3>
        <p>输入用户名参与抽奖</p>
        <p>点击中间按钮开始</p>
        <p>再次点击停止抽奖</p>
      </div>
      <LotteryGrid :is-drawing="isDrawing" @prize-selected="handlePrizeSelected" @click="isDrawing ? stopDraw() : startDraw()" />
      <div class="winners-list">
        <h3>中奖名单</h3>
        <ul>
          <li v-for="(winner, index) in winners" :key="index">
            {{ winner.name }} - {{ winner.prize }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  display: flex;
  place-items: center;
  min-width: 320px;
  min-height: 100vh;
  background: linear-gradient(to bottom, #ff4d4f, #fff2e8);
  font-family: Arial, sans-serif;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

.lottery-app {
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 10px;
  padding: 2rem;
  box-shadow: 0 0 20px rgba(255, 77, 79, 0.2);
}

h1 {
  color: #ff4d4f;
  font-size: 2.5em;
  margin-bottom: 1rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

.user-input {
  margin-bottom: 2rem;
}

input {
  padding: 0.5rem;
  font-size: 1rem;
  border: 2px solid #ff7a45;
  border-radius: 4px;
  outline: none;
}

input:focus {
  border-color: #ff4d4f;
  box-shadow: 0 0 5px rgba(255, 77, 79, 0.5);
}

.lottery-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.lottery-rules, .winners-list {
  flex-basis: 20%;
  background-color: #fff2e8;
  padding: 1rem;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(255, 122, 69, 0.1);
}

h3 {
  color: #ff4d4f;
  margin-top: 0;
  margin-bottom: 1rem;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 0.5rem;
  color: #ff7a45;
  font-weight: bold;
}
</style>