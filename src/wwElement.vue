<template>
  <div class="my-element" id="myElement">

    <p :style="textStyle">I am a custom element hello really sdsd sad!</p>
    <div class="color-picker">
        <div v-for="(color, idx) in colors" :key="idx" class="color-box" :style="{ backgroundColor: color }" @click="changeColor(color)"></div>
    </div>
    <canvas @mousedown.stop="startPainting" @mouseup.stop="finishedPainting" @mousemove.stop="draw" ref="canvas"></canvas>
  </div>
</template>

<script>
    import { ref, computed, defineProps, onMounted } from 'vue'

    export default {
      setup() {
        const props = defineProps({
          content: Object,
        })

        const count = ref(0)
        const painting = ref(false);
        const canvas = ref(null);
        const ctx = ref(null);
        const colors = ref(["#000000", "#FF0000", "#00FF00", "#0000FF", "#FFFF00", "#FF00FF", "#00FFFF"]);

        const textStyle = computed(() => {
          return '#546351'
        })

        const changeColor = (color) => {
          ctx.value.strokeStyle = color;
        };

        const draw = (e) => {
          if (!painting.value) return;
    
    
          ctx.value.lineTo(e.clientX - canvas.value.offsetLeft, e.clientY - canvas.value.offsetTop);
          ctx.value.stroke();
    
          ctx.value.beginPath();
          ctx.value.moveTo(e.clientX - canvas.value.offsetLeft, e.clientY - canvas.value.offsetTop);
        };

        const startPainting = (e) => {
          painting.value = true;
          draw(e);
        };

        const finishedPainting = () => {
          painting.value = false;
          ctx.value.beginPath();
        };

        onMounted(() => {
          canvas.value.width = 900;
          canvas.value.height = 500;
          console.log(canvas);
          ctx.value = canvas.value.getContext("2d");
          ctx.value.strokeStyle = "#f0b71a";
          ctx.value.lineWidth = 8;
          ctx.value.lineCap = "round";
        });

        return {
          textStyle,
          canvas,
          ctx,
          draw,
          colors,
          startPainting,
          finishedPainting,
          changeColor,
        }
      }
    }
</script>

<style lang="scss" scoped>
.my-element {
  p {
    font-size: 18px;
  }
}
.color-picker {
  display: flex;
  justify-content: center;
  margin-bottom: 1rem;
}
canvas {
  display: block;
  background-color: #ffffff;
  border: 2px solid #333;
  border-radius: 5px;
  cursor: crosshair;
}

.clear-button {
  display: block;
  margin: 1rem auto;
  padding: 0.5rem 1rem;
  background-color: #333;
  color: #fff;
  text-align: center;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-weight: bold;
}

.color-box {
  display: inline-block;
  width: 20px;
  height: 20px;
  margin: 0 5px;
  cursor: pointer;
  border-radius: 50%;
}
</style>
