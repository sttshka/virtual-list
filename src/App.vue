<script setup>
import {onUnmounted, ref, shallowRef} from "vue";
import { useVirtualList} from '@vueuse/core'

const isVirtualListShow = ref(true);
const rowsCount = shallowRef(Math.floor(Math.random() * 9000 + 100))
const cellsInRow = shallowRef(Math.floor(Math.random() * 10 + 10))

const data = shallowRef(Array
    .from(Array(rowsCount.value).keys(),
        () => Array.from(Array(cellsInRow.value).keys(),
            () => Math.floor(Math.random() * 100))))

const {list, containerProps, wrapperProps} = useVirtualList(data, {
  itemHeight: 96,
})

const interval = setInterval(() => {
  list.value.forEach((item) => {
    item.data.splice(Math.floor(Math.random() * item.data.length), 1, setRandomValue())
  })
}, 1000)

onUnmounted(() => {
  clearInterval(interval)
})

function setRandomValue() {
  return Math.floor(Math.random() * 100)
}
</script>

<template>
  <div class="info-banner">
      <span>rowsCount: {{rowsCount}}</span>
      <span>cellsInRow: {{cellsInRow}}</span>

    <button @click="isVirtualListShow = !isVirtualListShow">Toggle virtualList</button>
  </div>

  <div v-bind="containerProps" class="scrollContainer" v-if="isVirtualListShow">
    <div v-bind="wrapperProps" class="wrapperContainer" >
      <div class="row" v-for="{index, data} in list" :key="index">
        <div class="cells-wrapper">
          <div class="cell" v-for="(value, index) in data" :key="index" @click="data[index] = setRandomValue()">{{value}}</div>
        </div>
      </div>
    </div>
  </div>

  <div v-else>
    <div class="row" v-for="(item, index) in data" :key="index">
      <div class="cells-wrapper">
        <div class="cell" v-for="(value, index) in item" :key="index">{{value}}</div>
      </div>
    </div>
  </div>
</template>

<style>

.info-banner {
  border-radius: var(--border-radius);
  position: fixed;
  z-index: 100;
  left: var(--space-10);
  top: var(--space-10);
  background: #213547;
  color: white;
  padding: var(--space-10);
  display: flex;
  flex-direction: column;
  gap: var(--space-10);
}

.scrollContainer {
  height: calc(100vh - 4rem);
  margin: 2rem;
  border-radius: var(--border-radius);
}


.row {
  border-radius: var(--border-radius);
  height: 96px;
  margin-bottom: var(--space-10);
}

.cells-wrapper {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  gap: var(--space-10);
}

.cell {
  cursor: pointer;
  background: #535bf2;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 900;
  width: 100%;
  height: 100%;
  border-radius: var(--border-radius);
  color: white;
  transition: scale 0.2s ease-in-out;
}
.cell:hover {
  scale: .8;
  background: #3b42d1;
}
</style>
