<template>
  <div class="row items-center justify-center" :style="`padding: ${padding}`">
    <template v-for="(char, i) in chars">
      <clock-number-component
        v-if="char != ':'"
        :key="`${i}_${char}`"
        :char="parseInt(char)"
        :pixel-length="pixelLength"
        :night-mode="nightMode"
      />
      <clock-colon-component
        v-else
        :key="`${char}`"
        :pixel-length="pixelLength"
        :night-mode="nightMode"
        v-show="$q.screen.gt.xs"
      />
    </template>
  </div>
</template>

<script setup lang="ts">
import { useQuasar } from 'quasar';
import { computed, onBeforeUnmount, ref, watch } from 'vue';
import ClockColonComponent from './ClockColonComponent.vue';
import ClockNumberComponent from './ClockNumberComponent.vue';

const $q = useQuasar();

let hour = ref(-1);
let minute = ref(-1);
let padding = ref('0');

updateClock();

const interval = setInterval(updateClock, 1000);

function updateClock() {
  const now = new Date();
  hour.value = now.getHours();
  minute.value = now.getMinutes();
}

const chars = computed(() => {
  let fullTime = (hour.value < 10 ? '0' : '') + String(hour.value);
  fullTime += ':';
  fullTime += (minute.value < 10 ? '0' : '') + String(minute.value);

  return fullTime.split('');
});

const pixelLength = computed(() => {
  const cols = $q.screen.lt.sm ? 13 : 25;
  const rows = $q.screen.lt.sm ? 19 : 11;
  const appWidth = $q.screen.width;
  const appHeight = $q.screen.height;

  return appWidth / appHeight < cols / rows
    ? Math.floor(appWidth / cols)
    : Math.floor(appHeight / rows);
});

const nightMode = computed(() => hour.value > 18 || hour.value < 5);

watch(minute, (value) => {
  let length = pixelLength.value;

  const ciclePaddings = [
    `${length}px 0 0 ${length}px`,
    '0',
    `0 ${length}px ${length}px 0`,
    '0',
  ];

  padding.value = ciclePaddings[(value + 1) % 4];
});

onBeforeUnmount(() => {
  clearInterval(interval);
});
</script>
