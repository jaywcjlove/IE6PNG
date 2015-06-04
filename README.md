# IE6 PNG透明方案搜集

“珍惜生命，远离IE6”，IE6中的bug令很多Web前端开发人员实为头疼，因此不知道烧了多少脑细胞，在众多的Bug中最令人抓狂的就是IE对png图片的不支持，导致设计师和重构师放弃了很多很炫的效果，为了让大家在这方面少烧点脑细胞，把当前最流行最实用的IE6支持PNG图片的方案整理到我的仓库中，以前有搜集最近刚好有项目需要又到处找，顿时短路了，好久没有亲密接触IE6了！

http://www.w3cfuns.com/thread-297-1-1.html

## 滤镜解决方案

> 滤镜从IE4.0被微软正式引入，所以我们可以使用滤镜解决IE6的PNG透明问题，滤镜不仅可以实现目前CSS3的一些旋转效果而且还可以引入图片。注意：此方法在部分版本的IETest中无效，建议使用标准的IE6来进行测试！  


```css
#pics{
    background:url(../img/jslite.png) no-repeat;
    /*以下为IE6设置PNG透明代码*/
    _background:none;
    _filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src="img/jslite.png");
}
```

提示：如果需要使其支持链接的hover，那么需要在CSS中定义：`cursor:pointer`;使其呈现手型，否则将为默认的鼠标状态。  

注意：图片路径  


### 优点：
1. 绿色无插件；
2. 效率高，速度快；
3. 网速慢的时候，不会出现先灰底再透明的情况，支持远程图片；
4. 支持Hover等伪类，但是得使用两张图片，网速慢的情况下，会导致第二张图片暂时无法显示，因为还没有完全载入；


### 缺点：
1. 不支持平铺，虽然 filter 有 `sizingMethod="scale"`, 拉伸缩放模式，但是图片会变形，如果单纯的颜色或简单的渐变色还能横向平铺；
2. 不支持Img标签；
3. 不支持`CSS Sprite`；


### 使用情况：

1. 当没有img引入png时可考虑；
2. 当没有`CSS Sprite`需求时可考虑；
3. 当没有平铺需求时候可考虑；



## HTC插件解决方案

> 从IE 5.5版本开始，Internet Explorer（IE）开始支持Web 行为的概念。这些行为是由后缀名为.htc的脚本文件描述的，它们定义了一套方法和属性，程序员几乎可以把这些方法和属性应用到HTML页面上的任何元素上去。
