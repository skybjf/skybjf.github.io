---
layout:     post
title:      "canvas 动画"
subtitle:   "结合css sprite制作canvas动画"
date:       2015-10-16
author:     "小久哥"
header-img: "img/article/tpbg/canvas.jpg"
tags:
    - canvas
    - html
---

> canvas动画，请求一张大图，尽我所能的分析些利弊。基础的canvas画图之类的不做介绍了。

***

### 起因
在和公司的flash姐姐合作的时候，发现，那种重复动作的动画，总是要请求多张图片。处女座病犯了……这样的话会增加请求次数，影响加载速度呀！PS：用flash转canvas简直是神器，分分钟完成一个相当复杂的动画！

> 基础的canvas画图之类的不做介绍了，默认你会canvas画图的方法了(就算你不会，先看完，再决定你要不要学canvas)
> 默认你知道什么是css sprite(css图片精灵，也有叫 雪碧图 的，可以搜索下)

***

### 例子
[测试网页](http://skybjf.github.io/demo/20151016/canvas.html){:target="_blank"}

### 解释
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
* requestAnimationFrame和cancelAnimationFrame

在js代码的第一段是为了向下兼容浏览器用的，因为requestAnimationFrame()只支持现代浏览浏览器。chrome 31+，firefox 26+，ie 10+，opera 19+，safari 6+。
所以，如果你确定用户的浏览器在这个版本之上，就不用加那段兼容代码了。之后的代码没什么好说的，该注释的地方也做了注释。

有关 requestAnimationFrame 的一篇文章[基于脚本的动画的计时控制](https://technet.microsoft.com/zh-cn/library/hh920765.aspx){:target="_blank"}
requestAnimationFrame的用法和setTimeout比较像，但是他是针对动画的，省一些性能~~
cancelAnimationFrame这个是用来取消重画的方法，在测试的网页中，每执行一次requestAnimationFrame，动作就会快一倍，所以···就需要来cancel相同的次数才会停下来~

### 分析
在制作动画时，无论是采用多张图片还是采用一张大图，两个各有利弊。这个和csssprite有关。
本文中介绍的这种方法，弊端之一就是在还原动画上。真正的动画，可能时间帧是不均匀的，但是在本文中的这种方法，是按匀速来做的。
在测试页面里对比两个动画就会发现还是有些差别的，只能说是动画近似一样，没有实现1:1完全还原,通过flash导出的canvas好像也不能实现完全一样，不过在时间控制上比本文的方法要好些~


参考文献：

* [在线多张图片合成大图的工具](http://instantsprite.com/){:target="_blank"}
* [在线图片压缩工具](https://tinypng.com/){:target="_blank"}
* [requestAnimationFrame的定义 (W3C官方定义)](http://www.w3.org/TR/animation-timing/){:target="_blank"}
* [CREATE A SPRITE ANIMATION WITH HTML5 CANVAS AND JAVASCRIPT(本文代码在他的基础上做了修改)](http://www.williammalone.com/articles/create-html5-canvas-javascript-sprite-animation/){:target="_blank"}
