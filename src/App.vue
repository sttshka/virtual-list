<script setup>
import {onUnmounted, ref} from "vue";
import { useVirtualList} from '@vueuse/core'

const data = ref(Array
    .from(Array(110).keys(),
        () => Array.from(Array(11).keys(),
            () => Math.floor(Math.random() * 100))))

const {list, containerProps, wrapperProps} = useVirtualList(data, {
  itemHeight: 96,
})

const interval = setInterval(() => {
  list.value.forEach((item) => {
    item.data.splice(Math.floor(Math.random() * item.data.length), 1, Math.floor(Math.random() * 100))
  })
}, 1000)

onUnmounted(() => {
  clearInterval(interval)
})
</script>

<template>

  <div v-bind="containerProps" class="scrollContainer">
    <div v-bind="wrapperProps" class="wrapperContainer" >
      <div class="row" v-for="{index, data} in list" :key="index">
        <div class="cells-wrapper">
          <div class="cell" v-for="(value, index) in data" :key="index">{{value}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

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
