<template>
  <svg
    viewBox="0 0 4 28"
    :style="`margin: ${margin}px ${margin}px ${margin}px ${margin}px; width: ${pixelLength}px; height: ${height}px`"
  >
    <polygon
      :class="'active-' + (nightMode ? 'night' : 'day')"
      points="0,6 2,4 4,6 2,8"
    />
    <polygon
      :class="'active-' + (nightMode ? 'night' : 'day')"
      points="0,22 2,20 4,22 2,24"
    />
  </svg>
</template>

<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{ pixelLength: number; nightMode: boolean }>();

const margin = computed(() => Math.ceil(props.pixelLength / 2));
const height = computed(() => props.pixelLength * 7);
</script>
