<template>
  <div class="container">
    <h1 v-if="networks.length > 0" class="text-primary my-1">Datos de la red</h1>
    <div v-for="(network,index) in networks" :key="index" class="w-100 text-muted">
      <span class="d-block">SSID: {{netowork.ssid}}</span>
      <span class="d-block">MAC: {{netowork.mac}}</span>
      <span class="d-block">SIGNAL LEVEL: {{netowork.signal_level}}</span>
    </div>
    <div v-if="error" class="w-100 text-danger my-3">
      <h1 class="d-block">ERROR: {{ error }}</h1>
    </div>
  </div>
</template>

<script>
const wifi = require('node-wifi');

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      networks:[],
      error:undefined,
    }
  },
  mounted(){
    // Initialize wifi module
    // Absolutely necessary even to set interface to null
    wifi.init({
      iface: null // network interface, choose a random wifi interface if set to null
    });

    // Scan networks
    wifi.scan((error, networks) => {
      if (error) {
        this.error = error
      } else {
        this.networks = networks
            /*
            networks = [
                {
                  ssid: '...',
                  bssid: '...',
                  mac: '...', // equals to bssid (for retrocompatibility)
                  channel: <number>,
                  frequency: <number>, // in MHz
                  signal_level: <number>, // in dB
                  quality: <number>, // same as signal level but in %
                  security: 'WPA WPA2' // format depending on locale for open networks in Windows
                  security_flags: '...' // encryption protocols (format currently depending of the OS)
                  mode: '...' // network mode like Infra (format currently depending of the OS)
                },
                ...
            ];
            */
      }
    });
  }
}
</script>
