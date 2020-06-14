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
function findPeek(point1x,point1y,point2x,point2y) {
    var midx = (point1x + point2x)/2;
    var midy = (point1y + point2y)/2;
    var peekx = midx + 0.866 * (point1y - point2y);
    var peeky = midy + 0.866 * (point2x - point1x);
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
    var a = Math.sqrt((pointBx - pointAx) * (pointBx - pointAx) + (pointBy - pointAy) * (pointBy - pointAy));
    area += (a * a * Math.sqrt(3) / 36); 
    var thirdPoint = findThirdPoint(pointAx,pointAy,pointBx,pointBy);
    var fourthPoint = findFourthPoint(pointAx,pointAy,pointBx,pointBy);
    var peekPoint = findPeek(thirdPoint.x,thirdPoint.y,fourthPoint.x,fourthPoint.y);
   
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

  for(var depth = 1; depth <= 10; depth++) {
    p = area = 0;
    drawLine(-screenWidth/2, -screenHeight/2, screenWidth/2, screenHeight/2, depth);
    console.log('Depth = ', depth);
    console.log('Total perimeter = ', p);
    console.log('Total area = ', area);
    console.log('--------------------');
  }
```
