<template>
  <canvas ref="liveCanvas" style="width: 100%; height: 100%"></canvas>
</template>

<script>
import * as PIXI from "pixi.js";
import { Live2DModel } from "pixi-live2d-display/cubism4";

window.PIXI = PIXI; // 为了pixi-live2d-display内部调用

let app; // 为了存储pixi实例
let model; // 为了存储live2d实例

export default {
  props: {
    msg: String,
  },
  async mounted() {
    app = new PIXI.Application({
      view: this.$refs.liveCanvas,
      autoStart: true,
      resizeTo: window,
      backgroundAlpha: 0,
    });
    // 打包后live2d资源会出现在dist/下，这里用相对路径就能引用到了
    //1.https://cdn.jsdelivr.net/gh/guansss/pixi-live2d-display/test/assets/haru/haru_greeter_t03.model3.json
    //2.https://cdn.jsdelivr.net/gh/Eikanya/Live2d-model/Live2D/Senko_Normals/senko.model3.json

    model = await Live2DModel.from(
      "https://cdn.jsdelivr.net/gh/Eikanya/Live2d-model/Live2D/Senko_Normals/senko.model3.json"
    );
    app.stage.addChild(model);
    model.scale.set(0.3); // 调整缩放比例，一般原始资源尺寸非常大，需要缩小
    // 点击live2d人物不同部位时的回调，只有定义了可点击区域的人物才会收到回调
    model.on("hit", (hitAreas) => {
      // hitAreas 包含点击的区域和鼠标坐标
      console.log(hitAreas);
    });
  },
  beforeUnmount() {
    model?.destroy();
    app?.destroy();
  },
};
</script>

<style scoped></style>
