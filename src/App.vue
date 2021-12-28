<template>
  <div class="p-5 flex flex-col gap-3">
    <input v-model="width" />
    <div
      v-for="(chunk, i) in history"
      :key="i"
    >
      <div class="bg-stone-600 w-full h-2 mb-3"></div>
      <div class="flex flex-row flex-wrap gap-2">
        <div
          v-for="(field, j) in chunk"
          :key="j"
        >
          <Frame
            v-if="field !== 'error'"
            :field="field"
          />
          <div v-if="field === 'error'">Error</div>
        </div>
      </div>
    </div>
    <div>
      <div class="bg-stone-600 w-full h-2 mb-3"></div>
      <Frame
        :field="current"
        @field-click="onFieldClick"
        :clickable="true"
      />
    </div>
    <div class="flex flex-row gap-1">
      <button
        class="bg-green-800 rounded-md text-gray-50 p-2"
        @click="simulate"
      >
        Simulate
      </button>
      <button
        class="bg-red-500 rounded-md text-white p-2"
        @click="history = []"
      >
        Clear
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue'
import Frame from './Frame.vue'

const history = ref([])
const current = ref(new Array(15).fill(0))
const width = ref(15)

function onFieldClick(i, selected) {
  current.value[i] += selected ? 1 : -1
}

watch(width, (newWidth) => {
  if (!isNaN(parseInt(newWidth))) {
    current.value = new Array(parseInt(newWidth)).fill(0)
  }
})

function simulate() {
  const newHistoryChunk = []
  const errorValue = [current.value.slice(), 'error']
  let currentField = current.value.slice()
  newHistoryChunk.push(currentField)
  let newField = currentField.slice()
  let ok = false
  let iterations = 0
  while (!ok) {
    for (let i = 0; i < currentField.length; i++) {
      if (currentField[i] === 2) {
        newField[i] = 0
        newField[i - 1]++
        newField[i + 1]++
        if (i === 0 || i === currentField.length - 1) {
          history.value.push(errorValue)
          return
        }
      }
    }
    ok = newField.reduce((res, x) => res && x < 2, true)
    currentField = newField
    newHistoryChunk.push(currentField)
    newField = currentField.slice()
    iterations++
    if (iterations > 1000) {
      history.value.push(errorValue)
      return
    }
  }
  history.value.push(newHistoryChunk)
  current.value = newField
}
</script>
