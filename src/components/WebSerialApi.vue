<script setup lang="ts">
import { ref } from 'vue';

const isAvialable = 'serial' in navigator;

console.log(isAvialable);

navigator.serial.addEventListener('connect', (e) => {
  console.log('connect', e);
});

navigator.serial.addEventListener('disconnect', (e) => {
  console.log('disconnect', e);
});

const ports = ref(await navigator.serial.getPorts());

console.log('Ports', ports);

const requestSpecificPort = async () => {
  const port = await navigator.serial.requestPort();

  console.log('Port', port);
};

const readingData = async () => {
  const port = ports.value[0];

  const options = {
    baudRate: 115200,
    parity: 'none',
    stopbits: 1,
    databits: 8,
    handshake: 'none',
    rtsEnable: true,
  };

  await port.open(options);
  console.log('reading data start', port, port.readable);

  while (port.readable) {
    const reader = port.readable.getReader();

    try {
      while (true) {
        const { value, done } = await reader.read();

        if (done) {
          console.log('reading canceled!');
          break;
        }

        console.log('Data', value);
      }
    } catch (error) {
      console.log('reading error', error);
    } finally {
      reader.releaseLock();
    }
  }

  console.log('reading data finished');
};
</script>

<template>
  <h1>Web Serial API Examples</h1>

  <div>
    <button type="button" @click="requestSpecificPort">Request Port</button>
    <button tpye="button" @click="readingData">Reading Data...</button>
  </div>

  <h3>Currently Available Ports</h3>
  <div>
    {{ ports }}
  </div>
</template>
