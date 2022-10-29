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
      interval: null,
      mapElements: [],
    };
  },
  mounted() {
    const canvas = document.getElementById("canvas");
    canvas.style.width = window.innerWidth - 10 + "px";
    canvas.style.height = window.innerHeight - 10 + "px";
    this.context = canvas.getContext("2d");
    this.context.fillStyle = this.color;
    this.render();
  },
  methods: {
    render: function () {
      this.interval = setInterval(() => {
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
          if (this.elements.length < 1000) {
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
      // if (this.mapElements.includes(element)) {
      //   this.context.fillStyle = "white";
      //   this.context.fillRect(element.x, element.y, this.size, this.size);
      // } else {
      //   this.context.fillStyle = "black";
      //   this.context.fillRect(element.x, element.y, this.size, this.size);
      //   this.mapElements.push(element);
      // }
      this.context.fillRect(element.x, element.y, this.size, this.size);
    },
    addVenom(e) {
      const canvas = document.getElementById("canvas");
      const canvasWidth = canvas.offsetWidth;
      const canvasHeight = canvas.offsetHeight;
      const posX = e.clientX / (canvasWidth / 300);
      const posY = e.clientY / (canvasHeight / 150);
      for (let i = 0; i < 20; i++) {
        this.elements.unshift({ x: posX, y: posY });
      }
    },
  },
};
</script>

<template>
  <canvas @mousedown="addVenom" id="canvas"></canvas>
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
.center {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
