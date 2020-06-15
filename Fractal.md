# Fractal Image

``` <index.html>
<html>
    <body>
        <canvas id="myCanvas">
        </canvas>
        <script src="drawing.js"></script>
    </body>
</html>
```

``` <drawing.js>
var canvas = document.getElementById("myCanvas");
var ctx = canvas.getContext("2d");

canvas.height = window.innerHeight;
canvas.width = window.innerWidth;

var screenWidth = canvas.width;
var screenHeight = canvas.height;
var center = {
    x: canvas.width / 2,
    y: canvas.height / 2
};

var p = 0;
var area = 0;
function findPeekPoint(point1x,point1y,point2x,point2y) {
    var midx = (point1x + point2x)/2;
    var midy = (point1y + point2y)/2;
    var peekx = midx + Math.sqrt(3)/2 * (point1y - point2y);
    var peeky = midy + Math.sqrt(3)/2 * (point2x - point1x);
    return {
      x : peekx,
      y : peeky
    };
  }
function findThirdPoint(point1x,point1y,point2x,point2y){
  var x = (point2x+(2*point1x))/3;
  var y = (point2y+(2*point1y))/3;
  return {
    x: x,
    y: y
  };
}
function findFourthPoint(point1x,point1y,point2x,point2y){
  var x = (point1x+(2*point2x))/3;
  var y = (point1y+(2*point2y))/3;
  return {
    x: x,
    y: y
  };
}
  
function drawMyLine(sx, sy, ex, ey) {
  ctx.beginPath();
  ctx.moveTo(sx + screenWidth / 2, screenHeight / 2 - sy);
  ctx.lineTo(ex + screenWidth / 2, screenHeight / 2 - ey);
  ctx.strokeStyle = "#000000";
  ctx.stroke();
  p += Math.sqrt((ex-sx)*(ex-sx) + (ey-sy)*(ey-sy));
}
  
function drawLine(pointAx,pointAy,pointBx,pointBy,depth){
  var thirdPoint = findThirdPoint(pointAx,pointAy,pointBx,pointBy);
  var fourthPoint = findFourthPoint(pointAx,pointAy,pointBx,pointBy);
  var peekPoint = findPeekPoint(thirdPoint.x,thirdPoint.y,fourthPoint.x,fourthPoint.y);

  var a = Math.sqrt((pointBx - pointAx) * (pointBx - pointAx) + (pointBy - pointAy) * (pointBy - pointAy));
  area += (a * a * Math.sqrt(3) / 36); 

  if (depth == 1) {
    drawMyLine(pointAx,pointAy,thirdPoint.x,thirdPoint.y);
    drawMyLine(fourthPoint.x,fourthPoint.y,peekPoint.x,peekPoint.y);
    drawMyLine(peekPoint.x,peekPoint.y,thirdPoint.x,thirdPoint.y);
    drawMyLine(fourthPoint.x,fourthPoint.y,pointBx,pointBy);
    return;
  }


  drawLine(pointAx,pointAy,thirdPoint.x,thirdPoint.y, depth - 1);
  drawLine(peekPoint.x,peekPoint.y,fourthPoint.x,fourthPoint.y,depth - 1);
  drawLine(thirdPoint.x,thirdPoint.y,peekPoint.x,peekPoint.y,depth - 1);
  drawLine(fourthPoint.x,fourthPoint.y,pointBx,pointBy,depth - 1);
}

var a = screenHeight / 3;
var r = a / Math.sqrt(3);

var p1 = {
    x: 0,
    y: r
};
var p2 = {
    x: Math.cos(Math.PI / 6) * r,
    y: -1 * Math.sin(Math.PI / 6) * r 
}
var p3 = {
    x: -1 * Math.cos(Math.PI / 6) * r,
    y: -1 * Math.sin(Math.PI / 6) * r 
}

var depthMax = 8;
for(var depth = 1; depth < depthMax; depth++) {
  p = 0;
  area = Math.sqrt(3) * a * a / 4;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.font = "50px Arial";
  ctx.fillText("Koch Curve", canvas.width / 2 - 140, 200);

  drawLine(p1.x, p1.y, p2.x, p2.y, depth);
  drawLine(p2.x, p2.y, p3.x, p3.y, depth);
  drawLine(p3.x, p3.y, p1.x, p1.y, depth);

  console.log('============================================');
  console.log('Depth = ', depth);
  console.log('a = ', a);
  console.log('Total perimeter = ', p);
  console.log('Theoretical area =', Math.sqrt(3) * a * a / 4 * 8 / 5);
  console.log('Calculated area = ', area);
}
```
