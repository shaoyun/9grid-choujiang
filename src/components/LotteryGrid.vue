<script setup lang="ts">
import { ref, watch, onMounted, onUnmounted } from 'vue'

const props = defineProps<{
  isDrawing: boolean
}>()

const emit = defineEmits(['prizeSelected'])

const prizes = [
  '谢谢参与', '二等奖', '谢谢参与',
  '三等奖', '', '一等奖',
  '谢谢参与', '谢谢参与', '谢谢参与'
]

const rotationOrder = [0, 1, 2, 5, 8, 7, 6, 3]
const currentIndex = ref(0)
const intervalId = ref<number | null>(null)
const speed = ref(300)

const startRotation = () => {
  let rotationIndex = 0
  intervalId.value = setInterval(() => {
    currentIndex.value = rotationOrder[rotationIndex]
    rotationIndex = (rotationIndex + 1) % rotationOrder.length
  }, speed.value)
}

const stopRotation = () => {
  if (intervalId.value !== null) {
    const slowDown = setInterval(() => {
      speed.value += 50
      if (speed.value >= 1000) {
        clearInterval(slowDown)
        clearInterval(intervalId.value!)
        intervalId.value = null
        emit('prizeSelected', prizes[currentIndex.value])
      } else {
        clearInterval(intervalId.value!)
        startRotation()
      }
    }, 100)
  }
}

watch(() => props.isDrawing, (newValue) => {
  if (newValue) {
    speed.value = 300
    startRotation()
  } else {
    stopRotation()
  }
})

onMounted(() => {
  if (props.isDrawing) {
    startRotation()
  }
})

onUnmounted(() => {
  if (intervalId.value !== null) {
    clearInterval(intervalId.value)
  }
})
</script>

<template>
  <div class="lottery-grid">
    <div
      v-for="(prize, index) in prizes"
      :key="index"
      :class="['grid-item', `item-${index}`, { active: index === currentIndex }]"
    >
      {{ prize || (index === 4 ? (isDrawing ? '暂停' : '立即抽奖') : '') }}
    </div>
  </div>
</template>

<style scoped>
.lottery-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  width: 300px;
  height: 300px;
  margin: 0 auto;
  background-color: #ff4d4f;
  padding: 10px;
  border-radius: 10px;
}

.grid-item {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  font-size: 0.9rem;
  font-weight: bold;
  transition: all 0.3s;
  color: #fff;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  border: 3px solid transparent;
}

.grid-item.active {
  transform: scale(1.05);
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.7);
  border-color: #fff;
  animation: pulse 0.5s infinite alternate;
}

@keyframes pulse {
  from {
    box-shadow: 0 0 15px rgba(255, 255, 255, 0.7);
  }
  to {
    box-shadow: 0 0 25px rgba(255, 255, 255, 0.9);
  }
}

.item-0, .item-2, .item-6, .item-7, .item-8 {
  background-color: #ffa940;
}

.item-1, .item-3, .item-5 {
  background-color: #ff7a45;
}

.item-4 {
  background-color: #f5222d;
  font-size: 1.1rem;
}

.item-1, .item-3, .item-5 {
  font-size: 1rem;
}
</style>