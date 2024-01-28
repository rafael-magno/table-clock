<template>
  <svg
    viewBox="0 0 160 280"
    :style="`margin: ${margin}px ${margin}px ${margin}px ${margin}px; width: ${width}px; height: ${height}px`"
  >
    <polygon
      v-for="(points, i) in polygons"
      :key="i"
      :class="getClass(i)"
      :points="points"
    />
  </svg>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';

const props = defineProps<{
  char: number;
  pixelLength: number;
  nightMode: boolean;
}>();

const polygons = [
  '41,0 21,20 41,40 119,40 139,20 119,0',
  '20,21 0,41 0,119 20,139 40,119 40,41',
  '140,21 120,41 120,119 140,139 160,119 160,41',
  '41,120 21,140 41,160 119,160 139,140 119,120',
  '20,141 0,161 0,239 20,259 40,239 40,161',
  '140,141 120,161 120,239 140,259 160,239 160,161',
  '41,240 21,260 41,280 119,280 139,260 119,240',
];

const charPixels = [
  [1, 1, 1, 0, 1, 1, 1], // 0
  [0, 0, 1, 0, 0, 1, 0], // 1
  [1, 0, 1, 1, 1, 0, 1], // 2
  [1, 0, 1, 1, 0, 1, 1], // 3
  [0, 1, 1, 1, 0, 1, 0], // 4
  [1, 1, 0, 1, 0, 1, 1], // 5
  [1, 1, 0, 1, 1, 1, 1], // 6
  [1, 0, 1, 0, 0, 1, 0], // 7
  [1, 1, 1, 1, 1, 1, 1], // 8
  [1, 1, 1, 1, 0, 1, 1], // 9
];
const charPixel = ref(charPixels[props.char]);

const margin = computed(() => Math.ceil(props.pixelLength / 2));
const width = computed(() => props.pixelLength * 4);
const height = computed(() => props.pixelLength * 7);

function getClass(i: number) {
  let className = charPixel.value[i] ? 'active' : 'inactive';
  className += '-' + (props.nightMode ? 'night' : 'day');

  return className;
}
</script>
