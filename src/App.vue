<script setup lang="ts">
import WebSerialApi from './components/WebSerialApi.vue';
import WebBluetoothApi from './components/WebBluetoothApi.vue';
</script>

<template>
  <Suspense>
    <WebSerialApi />
    <template #fallback> Loading Web Serial API Examples... </template>
  </Suspense>
  <Suspense>
    <WebBluetoothApi />
    <template #fallback> Loading Web Bluetooth API Examples... </template>
  </Suspense>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
