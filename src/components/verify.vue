<!-- verify-demo -->
<template>
  <div id="verifyCanvas">
    <div>
      <canvas id="canvas" ref="canvas" width="120" height="40"></canvas>
      <button @click="reDraw" style="display:block;margin:auto;">换一张</button>
    </div>
    <input type="text" v-model="inputCode" />
    <button @click="submitCode">校验验证码</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      canvas: null,
      ctx: null,
      code: '',
      inputCode: ''
    };
  },
  mounted() {
    this.draw()
  },

  methods: {
    getRandomColor() {
      const randomColor = Math.floor(Math.random() * 16777215).toString(16);
      return "#" + randomColor;
    },
    draw() {
      let canvas = document.getElementById('canvas');
      let ctx = canvas.getContext('2d');
      // 绘制矩形
      ctx.strokeStyle = 'blue';
      ctx.strokeRect(0, 0, canvas.width, canvas.height);
      this.canvas = canvas;
      this.ctx = ctx;
      this.drawLine(canvas, ctx);
      this.drawDot(canvas, ctx);
      this.drawCode(canvas, ctx);
    },
    drawLine(canvas, ctx) {
      // 随机绘制8条直线
      for (let i = 0; i < 8; i++) {
        ctx.beginPath();
        ctx.moveTo(Math.random() * canvas.width, Math.random() * canvas.height);
        ctx.lineTo(Math.random() * canvas.width, Math.random() * canvas.height);
        ctx.strokeStyle = this.getRandomColor();
        ctx.stroke();
      }
    },
    drawDot(canvas, ctx) {
      // 随机生成20个小点，颜色随机
      // 圆的中心点在 (0-120, 0-40) 之间，半径是1个像素
      // let ctx = this.ctx;
      // let canvas = this.canvas;
      for (let i = 0; i < 20; i++) {
        ctx.beginPath();
        ctx.arc(
            Math.random() * canvas.width, 
            Math.random() * canvas.height,
            1,
            0,
            360 * Math.PI / 180
        )
        ctx.fillStyle = this.getRandomColor();
        ctx.fill();
      }
    },
    drawCode(canvas, ctx) {
      // 随机生成4个字符验证码
      const codes = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'm', 'n', 'l', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
      let result = [];
      for (let i = 0; i < 4; i++) {
        ctx.save();
        const x = 20 + i * 20; // 每个文字间隔是20个像素
        const y = 20 + Math.random() * 10; // 垂直坐标是 20-30 个像素

        // 位移
        ctx.translate(x, y);

        // 随机数
        const index = Math.floor(Math.random() * codes.length);
        const txt = codes[index];
        ctx.font = 'bolder 25px 微软雅黑';
        ctx.fillStyle = this.getRandomColor();
        ctx.fillText(txt, 0, 0);

        ctx.restore();
        result.push(txt);
        this.code = result.join('').toLowerCase();
      }
    },
    reDraw() {
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
      this.draw();
    },
    submitCode() {
      let inputCode = this.inputCode.toLowerCase();
      if (inputCode === this.code) {
        alert('验证码正确');
      } else {
        alert('验证码错误');
      }
      this.reDraw();
    }
  }
}

</script>
<style scoped>
</style>