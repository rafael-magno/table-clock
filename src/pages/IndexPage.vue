<template>
  <q-page class="row items-center justify-evenly">
    <clock-component />
  </q-page>
</template>

<script setup lang="ts">
import ClockComponent from 'src/components/ClockComponent.vue';
import { onBeforeUnmount, onMounted } from 'vue';

let wakeLock: WakeLockSentinel;

onMounted(async () => {
  wakeLock = await navigator.wakeLock.request();
});

onBeforeUnmount(() => {
  wakeLock.release();
});
</script>
