---
title: Markdown进阶
date: 2019-03-21 00:16:21
categories:
- 技术
- Markdown
tags: 
- Markdown	
---

Markdown 嵌入图片、音频、视频
参考：[Typora 官方文档](http://support.typora.io/HTML/)

<!--more-->

#### 插入图片

![](https://ws3.sinaimg.cn/large/a74cb9c0ly1g18ijjjtdij208w08wmyn.jpg)



#### 插入音频

```
<audio id="audio" controls="" preload="none">
      <source id="mp3" src=".mp3">
</audio>
```

例子：

<audio id="audio" controls="" preload="none">
 <source id="mp3"     src="https://www.huangtiancai.xyz/oneindex/?/Music/%E6%98%8E%E6%97%A5%E6%99%B4%E3%82%8C%E3%82%8B%E3%81%8B%E3%81%AA%28Piano%20%26%20Strings%20Version%29.mp3">
    </audio>




#### 插入视频1

```
<video src="xxx.mp4" />
```


```html
<video src="http://structr.learn-anything.cn/video/道理/陈铭：像我这种老好人，根本没什么真朋友！不jue亲戚的人，根本没什么真亲戚！.mp4" />
```

效果：

<video src="http://structr.learn-anything.cn/video/道理/陈铭：像我这种老好人，根本没什么真朋友！不jue亲戚的人，根本没什么真亲戚！.mp4" />



#### 插入视频2

```html
<video src="http://structr.learn-anything.cn/video/道理/陈铭：像我这种老好人，根本没什么真朋友！不jue亲戚的人，根本没什么真亲戚！.mp4" width="500" height="300"
controls="controls"></video> 
```

效果：

<video src="http://structr.learn-anything.cn/video/道理/陈铭：像我这种老好人，根本没什么真朋友！不jue亲戚的人，根本没什么真亲戚！.mp4" width="500" height="300"
controls="controls"></video> 


#### 插入视频3

```html
<video id="video" controls="" preload="none" 					  poster="http://media.w3.org/2010/05/sintel/poster.png">
      <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" type="video/mp4">
      <source id="webm" src="http://media.w3.org/2010/05/sintel/trailer.webm" type="video/webm">
      <source id="ogv" src="http://media.w3.org/2010/05/sintel/trailer.ogv" type="video/ogg">
      <p>Your user agent does not support the HTML5 Video element.</p>
</video>
```




<video id="video" controls="" preload="none"                        						     	     poster="http://media.w3.org/2010/05/sintel/poster.png">
      <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" 					 type="video/mp4">
      <source id="webm" src="http://media.w3.org/2010/05/sintel/trailer.webm" 					type="video/webm">
      <source id="ogv" src="http://media.w3.org/2010/05/sintel/trailer.ogv" 						type="video/ogg">
      <p>Your user agent does not support the HTML5 Video element.</p>
</video>




利用HTML5实现简单的纯粹的播放效果。其中有一些常用的标签属性。
### HTML5 Video标签的使用
常用的video标签：src、poster、preload、controls、width、height等，以及一个内部使用的标签<source>
#### src属性和poster属性
- `src`属性和`<img>`标签类似，这个属性用来指定视频的地址
- `poster`属性用于指定一张图片，在当前视频数据无效时显示（预览图）。视频数据无效可能是视频正在加载，可能是视频地址错误等等。  ***poster:海报、招贴***

```html
<video src="http://media.w3.org/2010/05/sintel/trailer.mp4"
       poster="http://media.w3.org/2010/05/sintel/poster.png">
</video>
```

<video src="http://media.w3.org/2010/05/sintel/trailer.mp4"
       poster="http://media.w3.org/2010/05/sintel/poster.png">
</video>



#### preload属性
此属性用于定义视频是否预加载,属性有三个可选择的值：`none`、`metadata`、`auto`。如果不使用此属性，默认为auto。
- None：不进行预加载。使用此属性值，可能是页面制作者认为用户不期望此视频，或者减少HTTP请求。
- Metadata：部分预加载。使用此属性值，代表页面制作者认为用户不期望此视频，但为用户提供一些元数据。
- Auto：全部预加载。

```html
<video src="http://media.w3.org/2010/05/sintel/trailer.mp4"
       poster="http://media.w3.org/2010/05/sintel/poster.png" preload="none">
</video>
```

<video src="http://media.w3.org/2010/05/sintel/trailer.mp4"
       poster="http://media.w3.org/2010/05/sintel/poster.png" preload="none">
</video>


#### controls属性

#### controls属性
Controls属性用于向浏览器指明页面制作者没有使用脚本生成播放控制器，需要浏览器启用本身的播放控制栏。
控制栏须包括播放暂停控制，播放进度控制，音量控制等等。
#### width属性和height属性
width属性为视频的宽度，height属性为视频的高度
#### source标签
Source标签用于给媒体（因为audio标签同样可以包含此标签，所以这儿用媒体，而不是视频）指定多个可选择的（浏览器最终只能选一个）文件地址，且只能在媒体标签没有使用src属性时使用。
浏览器按source标签的顺序检测标签指定的视频是否能够播放（可能是视频格式不支持，视频不存在等等），如果不能播放，换下一个。此方法多用于兼容不同的浏览器。Source标签本身不代表任何含义，不能单独出现。
此标签包含`src`、`type`、`media`三个属性。
- src属性：用于指定媒体的地址，和video标签的一样。
- Type属性：用于说明src属性指定媒体的类型，帮助浏览器在获取媒体前判断是否支持此类别的媒体格式。
- Media属性：用于说明媒体在何种媒介中使用，不设置时默认值为all，表示支持所有媒介。
```
<video id="video" controls="" preload="none" 					  
       poster="http://media.w3.org/2010/05/sintel/poster.png">
      <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" 
              type="video/mp4">
      <source id="webm" src="http://media.w3.org/2010/05/sintel/trailer.webm" 
              type="video/webm">
      <source id="ogv" src="http://media.w3.org/2010/05/sintel/trailer.ogv" 
              type="video/ogg">
      <p>Your user agent does not support the HTML5 Video element.</p>
</video>
```

<video id="video" controls="" preload="none" 					  
       poster="http://media.w3.org/2010/05/sintel/poster.png">
      <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" 
              type="video/mp4">
      <source id="webm" src="http://media.w3.org/2010/05/sintel/trailer.webm" 
              type="video/webm">
      <source id="ogv" src="http://media.w3.org/2010/05/sintel/trailer.ogv" 
              type="video/ogg">
      <p>Your user agent does not support the HTML5 Video element.</p>
</video>
