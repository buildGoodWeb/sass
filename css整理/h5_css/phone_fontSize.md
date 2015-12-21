##网易与淘宝的font-size思考前端(移动端)

http://www.html5cn.org/article-9159-1.html

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


