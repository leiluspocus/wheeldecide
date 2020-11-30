<template>
  <div id="wheel">
    <canvas id="wheel__triangle" width="50" height="50"></canvas>
    <canvas id="wheel__canvas" width="500" height="500"></canvas>
  </div>
</template>

<script>
export default {
  name: 'Wheel',
  props: {
      values: Array,
  },
  data() {
    return {
      deg: 0,
      slices: this.values.length,
      sliceDeg: 360/this.values.length,
      speed: 10,
      slowDownRand: 0,
      lock: false,
      isStopped: false,
      colors: ['#E0E2DB','#D2D4C8','#B8BDB5','#889696','#5F7470','#E0E2DB', "#D2D4C8", "#B8BDB5"]
    }
  },
  methods: {
    getCanvas() {
      return document.getElementById('wheel__canvas');
    },
    getWidth() {
      const canvas = this.getCanvas();
      return canvas.width;
    },
    getCenter() {
      return this.getWidth()/2;
    },
    deg2rad(deg) {
      return deg * Math.PI/180;
    },
    getCtx() {
      const canvas = this.getCanvas();
      return canvas.getContext('2d');
    },
    drawTriangle() {
      console.log('ttot');
      const context = document.getElementById('wheel__triangle').getContext('2d');
      // the triangle
      context.beginPath();
      context.moveTo(0, 0);
      context.lineTo(25, 50);
      context.lineTo(50, 0);
      context.closePath();

      // the outline
      context.lineWidth = 1;
      context.strokeStyle = '#F3FCF0';
      context.stroke();

      // the fill color
      context.fillStyle = "#FFD23F";
      context.fill(); 
    },
    rand(min, max) {
      return Math.random() * (max - min) + min;
    },
    drawSlice(deg, color) {
      const ctx = this.getCtx();
      const center = this.getCenter();
      const width = this.getWidth();
      ctx.beginPath();
      ctx.fillStyle = color;
      ctx.moveTo(center, center);
      ctx.arc(center, center, width/2, this.deg2rad(deg), this.deg2rad(deg+this.sliceDeg));
      ctx.lineTo(center, center);
      ctx.fill();
    },
    drawText(deg, text) {
      const ctx = this.getCtx();
      const center = this.getCenter();
      ctx.save();
      ctx.translate(center, center);
      ctx.rotate(this.deg2rad(deg));
      ctx.textAlign = "right";
      ctx.fillStyle = "#fff";
      ctx.font = 'bold 30px sans-serif';
      ctx.fillText(text, 130, 10);
      ctx.restore();
    },
    drawImg() {
      const ctx = this.getCtx();
      const width = this.getWidth();
      ctx.clearRect(0, 0, width, width);
      for(var i=0; i<this.slices; i++){
        this.drawSlice(this.deg, this.colors[i]);
        this.drawText(this.deg+this.sliceDeg/2, this.values[i]);
        this.deg += this.sliceDeg;
      }
    },
    anim() {
      this.deg += this.speed;
      this.deg %= 360;

      // Increment speed
      if(!this.isStopped && this.speed<3){
        this.speed = this.speed+1 * 0.1;
      }
      // Decrement Speed
      if(this.isStopped){
        if(!this.lock){
          this.lock = true;
          this.slowDownRand = this.rand(0.994, 0.998);
        } 
        this.speed = this.speed>0.2 ? this.speed*=this.slowDownRand : 0;
      }
      // Stopped!
      if(this.lock && !this.speed){
        var ai = Math.floor(((360 - this.deg - 90) % 360) / this.sliceDeg); // deg 2 Array Index
        ai = (this.slices+ai)%this.slices; // Fix negative index
        return console.log("You got:\n"+ this.values[ai] ); // Get Array Item from end Degree
      }

      this.drawImg();
      window.requestAnimationFrame( this.anim );
    },
    stopWheel() {
      this.isStopped = true;
    }
  },
  mounted() {
    this.deg = this.rand(0, 360);
    this.anim();
    this.drawTriangle();
    setTimeout(this.stopWheel, 500);
  }
}
</script>

<style scoped>
#wheel{
  display: flex;
  flex-direction: column;
  align-items: center;
} 

#wheel__triangle {
    position: absolute;
    top: 45px;
}
</style>