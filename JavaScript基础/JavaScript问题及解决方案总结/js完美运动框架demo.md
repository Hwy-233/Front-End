参考https://blog.csdn.net/u011175410/article/details/50351667  JS完美运动框架详解——原理分析及demo  
版权声明：本文为CSDN博主「波克比520」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。  
原文链接：https://blog.csdn.net/u011175410/article/details/50351667  

自我总结（更多内容参见以上链接：多物体运动、链式运动、同时运动）  
### 1.运动原理：改变DOM元素自身的位置属性如宽高、边距、透明度等，让DOM元素在web页面上运动起来  
#### 注意：运动的物体必须是绝对定位！  
    
### 2.运动框架：
#### a.先清除定时器：关闭之前的定时器（避免多次触发，调用多次定时器）  
#### b.开启定时器，计算速度  
#### c.判断停止条件，执行运动  
             
### 3.运动框架实例  
#### a.匀速运动：物体以一个【固定速度】做移动    
##### a-1.滑入滑出效果  
    <!DOCTYPE HTML>
    <html>
    
    <head>
        <meta charset="utf-8">
        <title>滑入滑出效果</title>
        <style>
            * {
                padding: 0;
                margin: 0;
            }
            
            #div {
                width: 200px;
                height: 200px;
                background-color: indianred;
                position: absolute;
                left: -200px;
                top: 0;
            }
            
            span {
                    width: 20px;
                height: 86px;
                background-color: gold;
                color: white;
                position: absolute;
                right: -20px;
                top: 50px;
            }
        </style>
    </head>
    
    <body>
        <!--   
        注意：span标签必须要放在div里面,因为它们是跟随父div一起运动的
    -->
        <div id="div">
            <span>滑动效果</span>
        </div>
    </body>
    <script>
        window.onload = function() {
            var oDiv = document.getElementById("div");
            var timer = null;
            var iSpeed = 0;
    
            div.onmouseover = function() {
                startMove(0); //鼠标移入时的目标位置是div.style.left=0的位置
            }
            div.onmouseout = function() {
                startMove(-200); //鼠标移出时的目标位置是div.style.left=-200的位置
            }
    
            function startMove(iTarget) {
                clearInterval(timer); //关闭之前的定时器,防止鼠标每移入div，就开一个定时器，累加的定时器造成运动混乱
                iSpeed = oDiv.offsetLeft < iTarget ? 10 : -10;
                timer = setInterval(function() {
                    if (oDiv.offsetLeft == iTarget) {
                        clearInterval(timer);
                    } else {
                        oDiv.style.left = oDiv.offsetLeft + iSpeed + "px";
                    }
                }, 30);
            }
        }
    </script>
    
    </html>
        
##### a-2.淡入淡出效果  
    <!DOCTYPE HTML>
    <html>
    
    <head>
        <meta charset="utf-8">
        <title>淡入淡出效果</title>
        <style>
            div {
                width: 200px;
                height: 200px;
                background-color: crimson;
                opacity: 0.3;
                filter: alpha(opacity=30);
                cursor: pointer;
            }
        </style>
    </head>
    
    <body>
        <div></div>
    </body>
    <script>
        window.onload = function() {
            var oDiv = document.getElementsByTagName("div")[0];
            var timer = null;
            var iSpeed = 0;
            var alpha = 30;
            oDiv.onmouseover = function() {
                startMove(100); //鼠标移入时的目标透明度是filter: alpha(opacity=100)
            };
            oDiv.onmouseout = function() {
                startMove(30); //鼠标移出时的目标透明度是filter: alpha(opacity=30)
            };
    
            function startMove(iTarget) {
                clearInterval(timer); //关闭之前的定时器,防止鼠标每移入div，就开一个定时器，累加的定时器造成运动混乱
                iSpeed = iTarget > alpha ? 10 : -10;
                timer = setInterval(function() {
                    if (alpha == iTarget) {
                        clearInterval(timer);
                    } else {
                        alpha += iSpeed;
                        oDiv.style.opacity = alpha / 100;
                        oDiv.style.filter = "alpha(opacity=" + alpha + ")";
                    }
                }, 30);
    
            }
        }
    </script>
    
    </html>
##### a-3.匀速运动的停止条件：距离足够近
    <!DOCTYPE HTML>
    <html>
    
    <head>
        <meta charset="utf-8">
        <title>匀速运动的停止条件：距离足够近</title>
        <style>
            #div1,
            #div2 {
                height: 300px;
                width: 2px;
                position: absolute;
                background-color: crimson;
            }
            
            #div1 {
                left: 100px;
            }
            
            #div2 {
                left: 300px;
            }
            
            #moveDiv {
                width: 100px;
                height: 100px;
                background-color: crimson;
                position: absolute;
                left: 600px;
                top: 140px;
            }
        </style>
    </head>
    
    <body>
        <p>
            <button>运动到100</button>
            <button>运动到300</button>
        </p>
        <div id="div1"></div>
        <div id="div2"></div>
        <div id="moveDiv"></div>
    </body>
    <script>
        window.onload = function() {
            var btn = document.getElementsByTagName("button");
            var box = document.getElementById("moveDiv");
            var timer = null;
            var iSpeed = 0;
            btn[0].onclick = function() {
                startMove(100);
            }
            btn[1].onclick = function() {
                startMove(300);
            }
    
            function startMove(iTarget) {
                clearInterval(timer);
                iSpeed = box.offsetLeft > iTarget ? -7 : 7;
                timer = setInterval(function() {
                    if (Math.abs(box.offsetLeft - iTarget) <= 7) {
                        clearInterval(timer);
                        box.style.left = iTarget + "px";
                        console.log(iTarget);
                    } else {
                        box.style.left = box.offsetLeft + iSpeed + "px";
                    }
                }, 30);
            }
        }
    </script>
    
    </html>  
#### b.缓冲运动：物体以一个【变化的速度】做移动；每次累加的速度值变小，就会使整个物体看起来越来越慢，以至于最后停掉  
##### b-1.当前值离目标值越近，速度越慢  
##### 当前值离目标值越远，速度越快  
##### speed = (目标值－当前值)/10  
##### b-2.与目标点相差一点，需要进行判断  
##### 目标值>当前值，需要对速度向上取整，即speed = Math.ceil(speed)  
##### 目标值<当前值，需要对速度向下取整，即speed = Math.floor(speed)  
##### b-3.如果当前值＝目标值，清除动画  
  <!DOCTYPE HTML>
  <html>
  
  <head>
      <meta charset="utf-8">
      <title>缓冲运动</title>
      <style>
          #div1 {
              width: 2px;
              height: 300px;
              background-color: crimson;
              position: absolute;
              left: 100px;
          }
          
          #div2 {
              width: 150px;
              height: 150px;
              background-color: crimson;
              position: absolute;
              left: 500px;
              top: 120px;
          }
      </style>
  </head>
  
  <body>
      <p><button>缓冲运动到100</button></p>
      <div id="div1"></div>
      <div id="div2"></div>
  </body>
  <script>
      window.onload = function() {
          var btn = document.getElementsByTagName("button")[0];
          var box = document.getElementById("div2");
          var iSpeed = 0;
          var timer = null;
          btn.onclick = function() {
              startMove(100);
          };
  
          function startMove(iTarget) {
              clearInterval(timer);
              timer = setInterval(function() {
                  iSpeed = (iTarget - box.offsetLeft) / 10;
                  iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed); // 但凡用到缓冲运动，一定要用到向上/向下取整！
                  if (iTarget == box.offsetLeft) {
                      clearInterval(timer);
                  } else {
                      box.style.left = box.offsetLeft + iSpeed + "px";
                  }
              }, 30);
          }
      };
  </script>
  
  </html>  
  
    
    


