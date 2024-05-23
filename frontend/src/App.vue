<template>
  <div>
    <button @click="connect()">Connect</button>
    <canvas id="canvas" width="96" height="96" class="canvas"></canvas>
    <img src="./image.png"></img>
    <button @click="print()">Print</button>
  </div>
</template>

<script>
import { niimbotPrintImage, niimbotGetRFID } from './utils/cmds.js'
import { niimbotConnect } from './utils/client.js'
import { setAsyncResponse } from './utils/helpers.js'

export default {
  name: 'App',
  data() {
    return {

    };
  },
  mounted() {
    var canvas = document.getElementById('canvas');
    var ctx = canvas.getContext('2d');


    var img = new Image();
    img.src = './image.png'
    img.onload = () => {
      ctx.drawImage(img, 0,0)
    }
  },
  methods: {
    async connect() {
      console.log("Attempting to connect")
      let promise = niimbotConnect().then(bt => {
        bt["device"].addEventListener('gattserverdisconnected', { "once": true });

      }).then(_ => niimbotGetRFID());
      console.log("Did it wowork?")
    },
    print() {
      const canvas = document.getElementById('canvas');
      setAsyncResponse(niimbotPrintImage(canvas, 0, 0, canvas.width, canvas.height, 1, 5), "PRINT");
    }

  }
}

</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
