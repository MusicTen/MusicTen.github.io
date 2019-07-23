# 视频播放

## ckplayer

在我们编写项目时，也许会接触到一些视频的操作，普通视频流的话，例如Ogg、MPEG4、WebM这类后缀的文件，这几类文件都可以被h5的video标签解析，并不需要做解析操作，那么我们在项目中也会用到直播视频的格式，我在项目中就有涉及到直播的需求，是rtmp格式的直播视频流，起初我找了相关插件进行解析，有video.js插件包，还有ckplayer插件包。ckpalyer插件能很好的解析rtmp格式的直播视频流，但是需要做一些配置；

#### 第一步：

​	下载：http://www.ckplayer.com/down/

#### 第二步：

​	在自己的项目中进行引入，前提需要将ckplayer包放到项目中，之后直接引入ckplayer.js文件即可，所有依赖关系都是通过ckplayer.js进行查找，所以只需引入这一个文件

#### 第三步：

​	在body中添加视频dom容器

```html
<div class="video active" id="video1"></div>
```

#### 第四步：

​	实例化ckplayer（这里我的操作是写了一个函数，将配置参数放到了函数里，之后在进行实例化）

```javascript
function video(className,url){
   var videoObject = {
      container: className, // “#”代表容器的ID，“.”或“”代表容器的class
      variable: 'player', // 该属性必需设置，值等于下面的new chplayer()的对象
      autoplay: true, // 自动播放
      live: true, // 直播视频形式
      mobileCkControls: false,
      poster: 'pic/wdm.jpg', // 封面图片
      video: url // 视频地址
   };
   return videoObject;
};
var player=new ckplayer("容器","rtmp格式地址");
```

截止目前，所有工作就已完毕，一个简单的直播功能就搭建好了，ckplayer中提供了很多视频配置，具体配置请参照官网（一般配置是去掉播放器的logo，这里我就拿这一个功能进行举例）。

在ckplayer.js文件中，找到style配置参数，将style内部的loading和logo配置的部分代码注释掉就可以了。

本示例是直接调用了一个视频地址进行播放，这里的示例提供的是一个mp4文件，同时也支持flv,f4v,m3u8，也支持rtmp协议，播放器会自动选择使用html5还是flashplayer进行播放，html5播放器优先。

## video.js

#### 入门使用

引入video.js和video-js.css

```html
<link href="https://cdnjs.cloudflare.com/ajax/libs/video.js/7.3.0/video-js.min.css" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/video.js/7.3.0/video.min.js"></script>
```

使用video标签就像下面这样：
```html
<video id="example_video_1" class="video-js vjs-default-skin" controls preload="none" width="640" height="264" poster="http://vjs.zencdn.net/v/oceans.png">
	<source src="http://vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
</video>
```

videojs使用方式就是以类似的方式开始的，不过由于我们借助videojs对视频进行一些控制或制定

```javascript
var player = videojs('example_video_1',{
    muted: true,
	controls : true/false,
	height:300, 
	width:300,
	loop : true,
	// 更多配置.....
});
```

#### 常用事件

- 播放 this.play()
- 停止 – video没有stop方法，可以用pause 暂停获得同样的效果
- 暂停 this.pause()
- 销毁 this.dispose()
- 监听 this.on(‘click‘,fn)
- 触发事件this.trigger(‘dispose‘)

```javascript
var options = {};

var player = videojs('example_video_1', options, function onPlayerReady() {
  videojs.log(‘播放器已经准备好了!‘);

  // In this context, this is the player that was created by Video.js.
  // 注意，这个地方的上下文， this 指向的是Video.js的实例对像player
  this.play();

  // How about an event listener?<br>  // 如何使用事件监听？
  this.on('ended', function() {
    videojs.log('播放结束了!');
  });
});
```

#### 常用选项

- autoplay: true/false 播放器准备好之后，是否自动播放 【默认false】
- controls: true/false 是否拥有控制条 【默认true】,如果设为false ,那么只能通过api进行控制了。也就是说界面上不会出现任何控制按钮
- height: 视频容器的高度，字符串或数字 单位像素 比如： height:300 or height:'300px'
- width: 视频容器的宽度, 字符串或数字 单位像素
- loop: true/false 视频播放结束后，是否循环播放
- muted : true/false 是否静音
- poster: 播放前显示的视频画面，播放开始之后自动移除。通常传入一个URL
- preload: 预加载
  	‘auto‘ 自动
    	’metadata‘ 元数据信息 ，比如视频长度，尺寸等
    	‘none‘ 不预加载任何数据，直到用户开始播放才开始下载
- children: Array | Object 可选子组件 从基础的Component组件继承而来的子组件，数组中的顺序将影响组件的创建顺序哦。