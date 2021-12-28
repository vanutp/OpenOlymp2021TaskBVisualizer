<template>
  <div
    :class="'cell' + (clickable ? ' clickable' : '') + (selected ? ' selected' : '')"
    @click="onClick"
  >
  </div>
</template>

<script setup>

const props = defineProps({
  clickable: Boolean,
  selected: Boolean,
})
const emit = defineEmits(['cell-click'])
function onClick() {
  if (props.clickable) {
    emit('cell-click', !props.selected);
  }
}
</script>

<style scoped>
.selected {
  background-color: #2196F3;
}
.clickable {
  cursor: pointer;
}
.cell {
  width: 100px;
  height: 100px;
  border: 1px solid #aaa;
}
</style>
