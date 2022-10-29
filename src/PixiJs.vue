<script>
export default {
  data() {
    return {
      size: 1,
      step: 30,
      color: "black",
      frames: 60,
      elements: [{ x: window.innerWidth / 2, y: window.innerHeight / 2 }],
      provDead: 0.3,
      provtop: 0.3,
      provDown: 0.3,
      provLeft: 0.3,
      provRight: 0.3,
      interval: null,

      objElement: new PIXI.Graphics(),
      app: new PIXI.Application({
        width: window.innerWidth,
        height: window.innerHeight,
      }),
    };
  },
  mounted() {
    document.getElementById("main").appendChild(this.app.view);
    this.objElement.moveTo(window.innerWidth / 2, window.innerHeight / 2);
    this.objElement.lineStyle(this.size, "0xffffff");
    this.render();
  },
  methods: {
    render: function () {
      this.interval = setInterval(() => {
        this.travel();
      }, 1000 / this.frames);
      this.app.stage.addChild(this.objElement);
    },
    travel() {
      const newArryElements = [];
      for (const element of this.elements) {
        if (
          element.x > 0 &&
          element.x < window.innerWidth &&
          element.y > 0 &&
          element.y < window.innerHeight
        ) {
          if (this.elements.length < 1000) {
            //newTop
            const top = {
              ...element,
              y: element.y + this.step,
            };
            if (Math.random() < this.provtop) {
              newArryElements.push(top);
            }

            //down
            const down = {
              ...element,
              y: element.y - this.step,
            };
            if (Math.random() < this.provDown) {
              newArryElements.push(down);
            }

            //left
            const left = {
              ...element,
              x: element.x - this.step,
            };
            if (Math.random() < this.provLeft) {
              newArryElements.push(left);
            }

            //right
            const right = {
              ...element,
              x: element.x + this.step,
            };
            if (Math.random() < this.provRight) {
              newArryElements.push(right);
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
      // this.objElement.drawCircle(element.x, element.y, this.size);
      this.objElement.lineTo(element.x, element.y);
      this.objElement.moveTo(element.x, element.y);
    },
    addVenom(e) {
      const posX = e.clientX;
      const posY = e.clientY;
      for (let i = 0; i < 20; i++) {
        this.elements.unshift({ x: posX, y: posY });
      }
    },
  },
};
</script>

<template>
  <div @mousedown="addVenom" id="main"></div>
</template>
