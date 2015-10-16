---
layout:     post
title:      "canvas 动画"
subtitle:   "结合css sprite制作canvas动画"
date:       2015-10-16
author:     "小久哥"
header-img: "img/article/tpbg/npm.jpg"
tags:
    - canvas
    - html


<script type="text/javascript">
	(function() {
	    var lastTime = 0;
	    var vendors = ['ms', 'moz', 'webkit', 'o'];
	    for(var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
	        window.requestAnimationFrame = window[vendors[x]+'RequestAnimationFrame'];
	        window.cancelAnimationFrame = window[vendors[x]+'CancelAnimationFrame'] || window[vendors[x]+'CancelRequestAnimationFrame'];
	    };
	    if (!window.requestAnimationFrame){
	        window.requestAnimationFrame = function(callback, element) {
	            var currTime = new Date().getTime();
	            var timeToCall = Math.max(0, 16 - (currTime - lastTime));
	            var id = window.setTimeout(function() { callback(currTime + timeToCall); }, timeToCall);
	            lastTime = currTime + timeToCall;
	            return id;
	        };
	    };
	    if (!window.cancelAnimationFrame){
	        window.cancelAnimationFrame = function(id) {
	            clearTimeout(id);
	        };
	    };
	}());

	(function () {
		var flash, coinImage, canvas;
		// 为了下面可以清除循环定义
		var loopname;
		// 循环
		function loop(){
			loopname=window.requestAnimationFrame(loop);
			flash.update();
			flash.render();
		}
		// 取消循环
		function cancelloop(){
			window.cancelAnimationFrame(loopname);
		};

		// 初始化canvas
		function init(){
			flash.render();
		}

		// 
		function sprite (options) {
			var that = {},
			// 首帧位置
				frameIndex = 0,
			// 播放第几帧
				tickCount = 0,
			// 每秒播放帧数
				ticksPerFrame = options.ticksPerFrame || 0,
			// 总帧数字
				numberOfFrames = options.numberOfFrames || 1;
			
			that.context = options.context;
			that.width = options.width;
			that.height = options.height;
			that.image = options.image;
			
			that.update = function () {
	            tickCount += 1;
	            if (tickCount > ticksPerFrame) {
					tickCount = 0;
	                // If the current frame index is in range
	                if (frameIndex < numberOfFrames - 1) {	
	                    // Go to the next frame
	                    frameIndex += 1;
	                } else {
	                    frameIndex = 0;
	                }
	            }
	        };
			
			that.clear=function(){
				// Clear the canvas
				that.context.clearRect(0, 0, that.width, that.height);
			};

			that.render = function () {
				// Clear the canvas
				that.context.clearRect(0, 0, that.width, that.height);
				// Draw the animation
				that.context.drawImage(
					that.image,
					frameIndex * that.width / numberOfFrames,
					0,
					that.width / numberOfFrames,
					that.height,
					0,
					0,
					that.width / numberOfFrames,
					that.height
				);
			};
			
			return that;
		}
		
		// Get canvas
		canvas = document.getElementsByTagName("canvas")[0];
		canvas.width = 500;
		canvas.height = 500;
		
		// Create sprite sheet
		coinImage = new Image();	
		
		// set sprite
		flash = sprite({
			context: canvas.getContext("2d"),
			width: 8000,
			height: 500,
			image: coinImage,
			numberOfFrames: 16,
			ticksPerFrame: 4
		});
		
		// Load sprite sheet
		coinImage.addEventListener("load", init);
		coinImage.src = "http://skybjf.github.io/img/article/insert/20151016/yvdi.png";

		var stop=document.querySelector(".stop");
		var goon=document.querySelector(".goon");
		stop.onclick=function(){cancelloop(); };
		goon.onclick=function(){loop(); };
	}());
</script>
---

> canvas动画，请求一张大图，尽我所能的分析些利弊。基础的canvas画图之类的不做介绍了。

***

### 起因

在和公司的flash姐姐合作的时候，发现，那种重复动作的动画，总是要请求多张图片。处女座病犯了……这样的话会增加请求次数，影响加载速度呀！PS：用flash转canvas简直是神器，分分钟完成一个相当复杂的动画！

> 基础的canvas画图之类的不做介绍了，默认你会canvas画图的方法了(就算你不会，先看完，再决定你要不要学canvas)
> 默认你知道什么是css sprite(css图片精灵，也有叫 雪碧图 的，可以搜索下)

***

### 例子：
这个是gif图片哦~：
![img](/img/article/insert/20151016/yvdi.gif)

这个是canvas哦：
<canvas width="500" height="500"></canvas>
<button class="stop">slow</button>
<button class="goon">quik</button>
<!-- <script src="./yvdi.js"></script> -->


### 说明
js代码如下：
<pre>
	<code>

	(function() {
	    var lastTime = 0;
	    var vendors = ['ms', 'moz', 'webkit', 'o'];
	    for(var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
	        window.requestAnimationFrame = window[vendors[x]+'RequestAnimationFrame'];
	        window.cancelAnimationFrame = window[vendors[x]+'CancelAnimationFrame'] || window[vendors[x]+'CancelRequestAnimationFrame'];
	    };
	    if (!window.requestAnimationFrame){
	        window.requestAnimationFrame = function(callback, element) {
	            var currTime = new Date().getTime();
	            var timeToCall = Math.max(0, 16 - (currTime - lastTime));
	            var id = window.setTimeout(function() { callback(currTime + timeToCall); }, timeToCall);
	            lastTime = currTime + timeToCall;
	            return id;
	        };
	    };
	    if (!window.cancelAnimationFrame){
	        window.cancelAnimationFrame = function(id) {
	            clearTimeout(id);
	        };
	    };
	}());

	(function () {
		var flash, coinImage, canvas;
		// 为了下面可以清除循环定义
		var loopname;
		// 循环
		function loop(){
			loopname=window.requestAnimationFrame(loop);
			flash.update();
			flash.render();
		}
		// 取消循环
		function cancelloop(){
			window.cancelAnimationFrame(loopname);
		};

		// 初始化canvas
		function init(){
			flash.render();
		}

		// 
		function sprite (options) {
			var that = {},
			// 首帧位置
				frameIndex = 0,
			// 播放第几帧
				tickCount = 0,
			// 每秒播放帧数
				ticksPerFrame = options.ticksPerFrame || 0,
			// 总帧数字
				numberOfFrames = options.numberOfFrames || 1;
			
			that.context = options.context;
			that.width = options.width;
			that.height = options.height;
			that.image = options.image;
			
			that.update = function () {
	            tickCount += 1;
	            if (tickCount > ticksPerFrame) {
					tickCount = 0;
	                // If the current frame index is in range
	                if (frameIndex < numberOfFrames - 1) {	
	                    // Go to the next frame
	                    frameIndex += 1;
	                } else {
	                    frameIndex = 0;
	                }
	            }
	        };
			
			that.clear=function(){
				// Clear the canvas
				that.context.clearRect(0, 0, that.width, that.height);
			};

			that.render = function () {
				// Clear the canvas
				that.context.clearRect(0, 0, that.width, that.height);
				// Draw the animation
				that.context.drawImage(
					that.image,
					frameIndex * that.width / numberOfFrames,
					0,
					that.width / numberOfFrames,
					that.height,
					0,
					0,
					that.width / numberOfFrames,
					that.height
				);
			};
			
			return that;
		}
		
		// Get canvas
		canvas = document.getElementsByTagName("canvas")[0];
		canvas.width = 500;
		canvas.height = 500;
		
		// Create sprite sheet
		coinImage = new Image();	
		
		// set sprite
		flash = sprite({
			context: canvas.getContext("2d"),
			width: 8000,
			height: 500,
			image: coinImage,
			numberOfFrames: 16,
			ticksPerFrame: 4
		});
		
		// Load sprite sheet
		coinImage.addEventListener("load", init);
		coinImage.src = "http://skybjf.github.io/img/article/insert/20151016/yvdi.png";

		var stop=document.querySelector(".stop");
		var goon=document.querySelector(".goon");
		stop.onclick=function(){
			cancelloop();
		};
		goon.onclick=function(){
			loop();
		};
	}());
	</code>
</pre>


需要注意的是：
requestAnimationFrame和cancelAnimationFrame两个

#### 额外说明：

1. 本地安装和全局安装，两种安装方法安装的位置不一样，如果想方便一些的话可以都采用全局安装，没有什么问题
2. 虽然不是处女座……还是不喜欢有cache的存在，建议隔一段时间清理一下cache

#### 更多有关npm的信息，请参考:

* [npmjs.com](http://npmjs.com/){:target="_blank"} (各种可以安装的插件都在里面)
* [npm官方文档](https://docs.npmjs.com/){:target="_blank"} 