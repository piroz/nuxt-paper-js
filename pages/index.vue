<template>
  <div class="container">
    <div class="col-7">
      <p class="text">click path stroke</p>
      <canvas id="editor"></canvas>
    </div>
    <div class="col-3">
      <table class="table" style="width: 100%">
        <tbody>
          <tr>
            <th>area</th>
            <td>{{~~path.area}}</td>
          </tr>
          <tr>
            <th>bounds</th>
            <td>{{path.bounds}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import paper from "paper";

export default {
  data() {
    return {
      path: {}
    };
  },
  created() {
    paper.install(window);
  },
  mounted() {
    paper.setup("editor");
    this.startPaper();
  },
  methods: {
    startPaper() {
      paper.view.viewSize = [paper.view.size.width, window.innerHeight];
      paper.activate();

      let width = 100

      let rectangle = new paper.Rectangle(
        new paper.Point(paper.view.size.width / 2 - width / 2, window.innerHeight / 2 - width / 2),
        new paper.Size(width, width)
      );

      this.path = new paper.Path.Rectangle(rectangle);
      this.path.strokeColor = "black";
      this.path.strokeWidth = 5;

      paper.view.onClick = (e) => {
        let hit = paper.project.hitTest(e.point);

        if (hit && hit.item instanceof paper.Path) {
          this.path.bounds.width = width * 2
        } else {
          this.path.bounds.width = width
        }
      }
    }
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

#editor {
  width: 100%;
  height: 100%;
}

.text {
  position: absolute;
  top: 30%;
  left: 30%;
  animation-name: my-fade-in;
  animation-duration: 0.5s;
  animation-delay: 0.5s;
  animation-direction: alternate-reverse;
  animation-iteration-count: infinite;
  animation-timing-function: ease-out;
}

@keyframes my-fade-in {
  from {
    opacity: 0;
    transform: scale(1.2, 1.2)
  }
  to {
    opacity: 1;
    transform: scale(1, 1)
  }
}
</style>
