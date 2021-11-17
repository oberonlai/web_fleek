---
title: 從新學習HTML5 (六) Canvas
tags: []
id: '275'
categories:
  - - HTML5
---

使用 <canvas> 標籤來定義 Canvas 繪圖區塊，座標系統左上角為 (0,0)，往右 X 軸為正數，往下 Y 軸為正數。先用選擇器取得 canvas 元素，再用 getContext("2d") 來開始使用 canvas 各項的 API。

```
<canvas id="myCanvas"></canvas>
<script>
  var canvas = document.querySelector("#myCanvas");
  var content = canvas.getContext("2d");
</script>
```

content.moveTo(10,10) - 移動起點到座標 10,10 的位置

content.lineTo(400,10) - 移動終點到座標 400,10 的位置

content.stroke() - 繪製圖形

content.beginPath() - 重新開始起點

content.closePath() - 閉合點與點之間的連線

content.strokeRect(0,0,100,100) - 從座標 (0,0) 描繪寬高各為 100px 的長方形(正方形)

content.arc(x,y,r,0,Math.PI \*3/2, true) - 描繪圓形，座標、半徑、起始角度、2PI 是完整的圓、true 為逆時針方向繪製

content.translate(100,100) - 移動起始點到 (100,100)

content.arcTo() - 另外一種畫弧線的方式，取兩條線交叉點畫弧

content.strokeStyle = '' - 線條調色，可以使用顏色英文、十六進位、rgb、rgba

上色的話要先用 content.fillStyle = 指定顏色，再用 content.fill() 填滿