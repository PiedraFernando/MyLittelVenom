<script>
export default {
  data() {
    return {
      context: "",
      size: 1,
      step: 1,
      color: "black",
      frames: 60,
      elements: [{ x: 150, y: 70 }],
      provDead: 0.3,
      provtop: 0.3,
      provDown: 0.3,
      provLeft: 0.3,
      provRight: 0.3,
    };
  },
  mounted() {
    const canvas = document.getElementById("canvas");
    this.context = canvas.getContext("2d");
    this.context.fillStyle = this.color;
    this.render();
  },
  methods: {
    render: function () {
      setInterval(() => {
        this.travel();
      }, 1000 / this.frames);
    },
    travel() {
      const newArryElements = [];
      for (const element of this.elements) {
        if (
          element.x > 0 &&
          element.x < 300 &&
          element.y > 0 &&
          element.y < 150
        ) {
          if (this.elements.length < 1500) {
            //newTop
            if (Math.random() < this.provtop) {
              newArryElements.push({ ...element, y: element.y + this.step });
            }
            //dawn
            if (Math.random() < this.provDown) {
              newArryElements.push({ ...element, y: element.y - this.step });
            }
            //left
            if (Math.random() < this.provLeft) {
              newArryElements.push({ ...element, x: element.x - this.step });
            }
            //right
            if (Math.random() < this.provRight) {
              newArryElements.push({ ...element, x: element.x + this.step });
            }
            //muere
            if (Math.random() > this.provDead) {
              newArryElements.push(element);
            }
          } else {
            //muere
            if (Math.random() > this.provDead) {
              newArryElements.push(element);
            }
          }
        }
        this.drawElement(element);
      }
      this.elements = newArryElements;
    },
    drawElement(element) {
      this.context.fillRect(element.x, element.y, this.size, this.size);
    },
  },
};
</script>

<template>
  <canvas class="full" id="canvas"></canvas>

</template>

<style>
.border {
  border: solid 1px black;
}
.text-white {
  color: white;
}
.text-center {
  text-align: center;
}
.full {
  width: 95vw;
  height: 93vh;
}
.center {
  display: flex;
  justify-content: center;
  align-items: center;
}
.xl {
  width: 600px;
  height: 300px;
}
</style>
