##网易与淘宝的font-size思考前端(移动端)

http://www.html5cn.org/article-9159-1.html

###相关知识了解

1,*设备的物理分辨率(devicePixelRatio * scale)，在scale为1的情况下，device-width = 设备的物理分辨率/devicePixelRatio。* 

2,*devicePixelRatio称为设备像素比，每款设备的devicePixelRatio都是已知，并且不变的，目前高清屏，普遍都是2，不过还有更高的，比如2.5, 3 等 。*

### 网易做法

html标签下的font-size属性分析：

> deviceWidth = 320，font-size = 320 / 6.4 = 50px

> deviceWidth = 375，font-size = 375 / 6.4 = 58.59375px

> deviceWidth = 414，font-size = 414 / 6.4 = 64.6875px

> deviceWidth = 500，font-size = 500 / 6.4 = 78.125px

#### 原因：

> 基于iphone5做，分辨率为640，然后采取100为标准，得到640／100 ＝ 6.4

js设置html字体大小代码：

	document.documentElement.style.fontSize = document.documentElement.clientWidth / 6.4 + 'px';

#### 结果：

除了文本的font-size属性之外的其它css尺寸，比如width，height，line-height都使用了rem作为单位。

文本的font-size需要额外的媒介查询，并且font-size不能使用rem，设置：

	@media screen and (max-width:321px){
	    .m-navlist{font-size:15px}
	}
	@media screen and (min-width:321px) and (max-width:400px){
	    .m-navlist{font-size:16px}
	}
	@media screen and (min-width:400px){
	    .m-navlist{font-size:18px}
	11
	}

#### 注意：
1，如果采用网易这种做法，视口要如下设置：

	<meta name="viewport" content="initial-scale=1,maximum-scale=1, minimum-scale=1">

2，当deviceWidth大于设计稿的横向分辨率时，html的font-size始终等于横向分辨率/body元素宽。

稍微改一下就行了（html的font-size最大为100px）：

	var deviceWidth = document.documentElement.clientWidth;
	if(deviceWidth > 640) deviceWidth = 640;
	document.documentElement.style.fontSize = deviceWidth / 6.4 + 'px';

	
### 淘宝做法

> 淘宝触屏版布局的前提就是viewport的scale根据devicePixelRatio动态设置：

> 淘宝的设计稿是基于750的横向分辨率

	在devicePixelRatio为2的时候，scale为0.5
	在devicePixelRatio为3的时候，scale为0.3333
	
通过js设置viewport的方法如下：
	var scale = 1 / devicePixelRatio;
	document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');

html元素的font-size的计算公式，font-size = deviceWidth / 10

	document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
	
淘宝也设置了一个临界点，当设备竖着时横向物理分辨率大于1080时，html的font-size就不会变化了，原因也是一样的，分辨率已经可以去访问电脑版页面了。	
	
	


