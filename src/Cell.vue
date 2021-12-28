<template>
  <div
    :class="
      'w-20 h-20 border border-gray-400' +
      (clickable ? ' cursor-pointer' : '') +
      (selected ? ' bg-blue-500' : '')
    "
    @click="onClick"
  ></div>
</template>

<script setup>
const props = defineProps({
  clickable: Boolean,
  selected: Boolean,
})
const emit = defineEmits(['cell-click'])
function onClick() {
  if (props.clickable) {
    emit('cell-click', !props.selected)
  }
}
</script>
