IE中的CSS3不完全兼容方案
转自:http://www.cnblogs.com/platero/archive/2010/08/31/1870151.html

到Internet Explorer 8为止，IE系列是不支持CSS3的。在IE中要做一些常用的效果如圆角、阴影，就需要用一些冗余而无意义的元素和图片来做出这些效果。在厌倦这些后，就 想着用更为简洁、直接有效、CSS3式的办法来解决这些问题。好在就算是饱受批评的Internet Explorer，其本身也是足够强大的。IE特有的技术可以很好的实现一些CSS3的效果。

ie-css3

Opacity透明度
元素的透明度在IE中可以很方便的用滤镜来实现。

1	background-color:green;
2	opacity: .4;
3	filter:progid:DXImageTransform.Microsoft.alpha(opacity=40);
这里半透明区域
opacity: .4;

filter:alpha(opacity=40);

border-radius圆角/Box Shadow盒阴影/Text Shadow文字阴影
在IE中可以利用Vector Markup Language (VML)和javascript来实现这些效果，参见IE-CSS3，在引用了一个HTC文件后，在IE浏览器中就可以使用这三种CSS3属性了。

1	-moz-border-radius: 15px; /* Firefox */
2	-webkit-border-radius: 15px; /* Safari 、Chrome */
3	border-radius: 15px; /* Opera 10.5+, IE6+ 使用 IE-CSS3*/
4	-moz-box-shadow: 5px 5px 5px #000; /* Firefox */
5	-webkit-box-shadow: 5px 5px 5px #000; /* Safari、Chrome */
6	box-shadow: 5px 5px 50px #000; /* Opera 10.5+，IE6+ 使用 IE-CSS3 */
7	behavior: url(ie-css3.htc); /*引用ie-css3.htc */
实际上，在IE中有滤镜来实现阴影(shadow)和投影(drop-shadow)效果的

shadow会产生连续、渐变的阴影

1	filter: progid:DXImageTransform.Microsoft.Shadow(color='#000000', Direction=145, Strength=10);
drop-shadow产生的阴影没有明暗变化

1	filter:progid:DXImageTransform.Microsoft.DropShadow(Color="#6699CC",OffX="5",OffY="5",Positive="1");
 

滤镜似乎和现有的htc脚本有冲突，或者可以称之为特性：两者同时在一个元素上启用的时候，滤镜效果会转移到其子元素上

Gradients渐变
IE中提供了一个简单的渐变滤镜

1	background-image: -moz-linear-gradient(top, #444444, #999999); /* FF3.6 */
2	background-image: -webkit-gradient(linear,left top,left bottom,color-stop(0, #444444),color-stop(1, #999999)); /* Saf4+, Chrome */
3	filter:  progid:DXImageTransform.Microsoft.gradient(startColorStr='#444444', EndColorStr='#999999'); /* IE6+ */
在实现IE中的渐变很简单

RGBA透明度颜色背景
渐变滤镜支持RGBA的颜色，startColorStr和EndColorStr的前两位是Alpha通道值。使用带alpha通道来模拟RGBA颜色背景的同时，应该去掉其background-color属性。

1	background-color: rgba(0, 255, 0, 0.6); /* FF3+, Saf3+,Opera 10.10+, Chrome */
2	filter:progid:DXImageTransform.Microsoft.gradient(startColorStr='#9900FF00',EndColorStr='#9900FF00'); /* IE6+*/
Multiple Backgrounds多重背景图片
支持CSS3多重背景图片的浏览器中可以用下面的语句来实现背景多重图片：

1	background: url(bg-image-1.gif) center center no-repeat, url(bg-image-2.gif) top left;
要在IE中实现多背景图片，在需要在单独的IE hack样式表中加入下面的代码：

1	background: transparent url(bg-image-1.gif) top left repeat;
2	filter: progid:DXImageTransform.Microsoft.AlphaImageLoader (src='bg-image-2.gif', sizingMethod='crop'); /* IE6+ */
CSS3浏览器的多重背景

IE的多重背景

Tranformations/rotate旋转元素
IE中有两个滤镜可以实现元素的旋转，BasicImage和Matrix，前者简单方便但是只能做90*n(n∈{1,2,3,4})度的旋转；Matrix滤镜功能强大，但是参数复杂。

1	-moz-transform: rotate(180deg);  /* FF3.5+ */
2	-o-transform: rotate(180deg);  /* Opera 10.5 */
3	-webkit-transform: rotate(180deg);  /* Saf3.1+, Chrome */
4	 
5	filter:progid:DXImageTransform.Microsoft.BasicImage(rotation=2);
6	filter:progid:DXImageTransform.Microsoft.Matrix(sizingMethod='auto expand',M11=-1, M12=-1.2246063538223772e-16, M21=1.2246063538223772e-16, M22=-1);
旋转也很简单

@font-face服务器端字体
考虑到汉字字体的尺寸，这个CSS3的特性不算实用

1	font-family:'webFont';
2	src:url('myfont.eot');/*IE6+*/
3	 
4	src:local('fontname'),/*字体名称*/
5	url('myfont.woff') format('woff'),/*FF3.6*/
6	url('myfont.ttf') format('truetype');/*saf3+,chrome,FF3.5,opera10+*/
字体声明并引用了以后，可以在网页的其他地方用font-family使用这一字体。

可以在同一个元素上启用多个滤镜，如：

1	filter: progid:DXImageTransform.Microsoft.Shadow(color='#000000', Direction=145, Strength=5)progid:DXImageTransform.Microsoft.Alpha(opacity=40);
 

虽然一些用滤镜模仿CSS3的效果难称完美，但在一些情况下，运用这些技术能够让我们的代码更为简洁和统一
