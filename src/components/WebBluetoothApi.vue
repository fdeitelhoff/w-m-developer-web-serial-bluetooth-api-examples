<script setup lang="ts">
import { ref } from 'vue';

const isAvialable = 'bluetooth' in navigator;

console.log(isAvialable);

const devices = ref(await navigator.bluetooth.getDevices());

const requestSpecificDevice = async () => {
  // const options = { acceptAllDevices: true };
  const options = {
    filters: [{ namePrefix: 'BBC micro:bit' }],
    optionalServices: [
      '0000180a-0000-1000-8000-00805f9b34fb',
      'e95dd91d-251d-470a-a062-fa1922dfa9a8',
    ],
  };

  const device = await navigator.bluetooth.requestDevice(options);

  console.log('Device:', device.id, device.name, device.gatt);

  const server = await device.gatt.connect();

  console.log('server', server);

  const service = await server.getPrimaryService(
    '0000180a-0000-1000-8000-00805f9b34fb'
  );

  console.log(service);

  const characteristic = await service.getCharacteristic(
    '00002a24-0000-1000-8000-00805f9b34fb'
  );

  console.log(characteristic);

  const value = await characteristic.readValue();

  var dec = new TextDecoder('utf-8');
  console.log(value, dec.decode(value));

  const ledService = await server.getPrimaryService(
    'e95dd91d-251d-470a-a062-fa1922dfa9a8'
  );
  const ledTextCharacteristic = await service.getCharacteristic(
    'e95d93ee-251d-470a-a062-fa1922dfa9a8'
  );

  var enc = new TextEncoder();
  ledTextCharacteristic.writeValue(enc.encode('Test'));
};
</script>

<template>
  <h1>Web Bluetooth API Examples</h1>

  <div>
    <button type="button" @click="requestSpecificDevice">Request Device</button>
  </div>

  <h3>Currently Available Ports</h3>
  <div>
    {{ devices }}
  </div>
</template>
