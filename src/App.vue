
<template>
  <div id="app">
    <canvas
      id="myCanvas"
      :width="width"
      :height="height"
      style="border: 1px solid"
    ></canvas>
  </div>
</template>
<script>
export default {
  name: "app",
  data() {
    return {
      width: "700px",
      height: "700px",
    };
  },
  mounted() {
    let points = [
      { x: 200, y: 200 },
      { x: 400, y: 200 },
      { x: 300, y: 400 },
    ];
    let drag_point = -1;
    let canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");

    canvas.onmousedown = function (e) {
      var pos = getPosition(e);
      drag_point = getPointAt(pos.x, pos.y);
    };
    canvas.onmousemove = function (e) {
      if (drag_point != -1) {
        var pos = getPosition(e);
        points[drag_point].x = pos.x;
        points[drag_point].y = pos.y;
        redraw();
      }
    };
    canvas.onmouseup = function () {
      drag_point = -1;
    };

    function getPosition(event) {
      var rect = canvas.getBoundingClientRect();
      var x = event.clientX - rect.left;
      var y = event.clientY - rect.top;
      return { x, y };
    }

    function getPointAt(x, y) {
      for (var i = 0; i < points.length; i++) {
        if (
          Math.abs(points[i].x - x) < 100 &&
          Math.abs(points[i].y - y) < 100
        )
          return i;
      }
      return -1;
    }

    function redraw() {
      if (points.length > 0) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawLines();
        drawCircles();
      }
    }

    function drawLines() {
      ctx.beginPath();
      ctx.moveTo(points[0].x, points[0].y);
      ctx.strokeStyle = "blue";
      ctx.lineWidth = 2;
      points.forEach((p) => {
        ctx.lineTo(p.x, p.y);
      });
      ctx.stroke();
    }

    function drawCircles() {
      ctx.strokeStyle = "green";
      ctx.fillStyle = "green";
      ctx.lineWidth = 4;
      points.forEach((p) => {
        ctx.beginPath();
        ctx.rect(p.x - 50, p.y - 50, 100, 100);
        ctx.stroke();
        ctx.fill();
      });
    }

    redraw();
  },
};
</script>
<style>
</style>