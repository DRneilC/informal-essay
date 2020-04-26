偶然看到有些网站上会使用随机粒子的背景，感觉很炫，就自己试着写了一个，效果还不错。

#### html部分
```
<canvas id="myCanvas"></canvas>
```

#### js部分
```
let SCREEN_WIDTH = window.innerWidth, SCREEN_HIEGHT = window.innerHeight, mouseX = 0, mouseY = 0

let canvas = document.getElementById('myCanvas');
canvas.width = SCREEN_WIDTH;
canvas.height = SCREEN_HIEGHT;
canvas.style.backgroundColor = '#333'
let ctx = canvas.getContext('2d');

const points = [];
const colors = ['red', 'white', 'green', 'blue', 'yellow', 'pink', 'deeppink', 'lightblue', 'greenyellow', 'lightsalmon']
// 随机生成点的坐标，需指定radius的最大值
function getPoint(radius) {
    const r = +(Math.random() * radius).toFixed(4), // 粒子的半径
        x = Math.random() * (SCREEN_WIDTH - r) + r, // 粒子的x坐标
        y = Math.random() * (SCREEN_HIEGHT - r) + r, // 粒子的y坐标
        
        rateX = +(Math.random() * 2 - 1).toFixed(4), // 粒子在x方向运动的速率
        rateY = +(Math.random() * 2 - 1).toFixed(4), // 粒子在y方向运动的速率
        colornum = Math.round(Math.random()*10),
        color = colors[colornum]              
        
    return { x, y, r, rateX, rateY, color };
}

// 随机生成100个点的坐标信息
for (let i = 0; i < 100; i++) {
    points.push(getPoint(4));
}

function drawPoints() {                
    points.forEach((item, i) => {
        ctx.beginPath();
        ctx.arc(item.x, item.y, item.r, 0, Math.PI*2, false);

        ctx.fillStyle = item.color;
        ctx.fill();
        
        if(item.x >= item.r && item.x < SCREEN_WIDTH - item.r) {
            item.x += item.rateX;
        } else {
            item.rateX = item.rateX * -1
            item.x += item.rateX
        } 

        if(item.y >= item.r && item.y < SCREEN_HIEGHT - item.r) {                
            item.y += item.rateY;
        } else {
            item.rateY = item.rateY * -1
            item.y += item.rateY
        }                      
    });                
}


// 计算两点之间的距离
function cal(x1, y1, x2, y2) {
    var disX = Math.abs(x1 - x2), disY = Math.abs(y1 - y2);
    
    return Math.sqrt(disX * disX + disY * disY);
}

function drawLines() {
    const len = points.length;


    // 计算指针与附近的圆的距离
    for(let i = 0; i < len; i++) {
        const x1 = points[i].x,
            y1 = points[i].y,
            disPoint = cal(x1, y1, mouseX, mouseY);
    
        // 如果距离小于10，画线
        if(disPoint <= 150) {
            ctx.beginPath();
            ctx.moveTo(x1, y1);
            ctx.lineTo(mouseX, mouseY);
            ctx.strokeStyle = '#fff';
            ctx.lineWidth = 0.4;
            ctx.stroke();
        }
    }
}

window.addEventListener('mousemove', mouseHandler);

function mouseHandler (event){                
    mouseX = event.pageX
    mouseY = event.pageY     
}


function draw() {
    ctx.clearRect(0, 0, SCREEN_WIDTH, SCREEN_HIEGHT);
    drawPoints();
    drawLines();
    window.requestAnimationFrame(draw);
}

draw()
```

#### 效果图
![粒子.gif](https://upload-images.jianshu.io/upload_images/4241523-810d43c6d66ae3a7.gif?imageMogr2/auto-orient/strip)
