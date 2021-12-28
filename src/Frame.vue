<template>
  <div class="flex sflex-row">
    <div
      class="flex flex-col"
      v-for="(x, i) in field"
      :key="i"
    >
      <Cell
        :clickable="clickable"
        :selected="x >= 2"
        @cell-click="(sel) => onCellClick(i, sel)"
      />
      <Cell
        :clickable="clickable"
        :selected="x >= 1"
        @cell-click="(sel) => onCellClick(i, sel)"
      />
    </div>
  </div>
</template>

<script setup>
import Cell from './Cell.vue'

const emit = defineEmits(['field-click'])
const props = defineProps({
  field: Array,
  clickable: {
    type: Boolean,
    default: false,
  },
})

function onCellClick(x, selected) {
  emit('field-click', x, selected)
}
</script>
