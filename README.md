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

### 优点：

1. 一次性配置好，只需要像平时一样引入png图片，也不需要考虑png相对于html路径的问题，当目录有所变化，只需要修改htc文件或css中htc文件路径即可。
2. 支持平铺属性。
3. 不支持Img标签；
4. 不支持Hover等伪类；


### 缺点：

1. 多引入了js、图片和htc，共三个文件；
2. 不支持CSS Sprite；
3. 当文件载入之前，会先暂时呈现灰底；


使用情况：

1. 当没有img引入png时可考虑；
2. 当没有CSS Sprite需求时可考虑；
3. PNG图片比较频繁修改时可考虑；

## DD_belatedPNG解决方案

### 思路：
引入刚js文件，由于此js只有使用IE6时才有用，所以为了让我们的页面更加高效的执行，我们可以将上方代码修改如下，只有IE6的时候才调用执行此JavaScript：


```js
<!--[if lte IE 6]> 
<script type="text/javascript" src="js/DD_belatedPNG.js"></script> 
<script language="javascript" type="text/javascript">
      DD_belatedPNG.fix("#pngImg,#pics,#picsRepeat,#links,#link:hover");
</script>
<![endif]-->
```


```html
 <div id="pics">
    dsds<br><br><br><br><br><br><br>
 </div>
```


```css
#pics{background:url(img/jslite.png) repeat-x;}
```

### 说明：
1. 如果为链接和链接的hover设置透明，那么您按照下方这么来写，在部分版本里面可以不用加入:hover直接写选择器即可，但是为了保险，建议咱们还是加上:hover：
 `DD_belatedPNG.fix("#links,#link:hover");`
2. 如果页面中存在很多png，`DD_belatedPNG.fix();`函数的参数岂不是很长？我们可以使用这种写法：`DD_belatedPNG.fix(".pngFix,.pngFix:hover");`,使用上述的写法，我们的html中只需要在相对应的标签上加入`class="pngFix"`就行了，如果有多个类样式，按照平时的多个类样式的写法即可`class="abc cbc pngFix"`. 使用此方法的时候，我们每次都要加载两个js文件或者写两个`<script>`标签才行，这样不太好，http请求会增多，那么我们可以打开DD_belatedPNG.js文件，在尾部加入如下代码即可：


```js
window.onload = function(){
   DD_belatedPNG.fix(".pngFix,.pngFix:hover");
}
```

这样我们只需要引入此JS，在需要透明的标签上加入`class="pngFix"`即可，简单 · 方便 · 快捷！


### 优点：
1. CSS代码无需任何修改，按照平时的思路来写即可；
2. 无需配置；
3. 没有多余的gif图片；
4. 支持img；
5. 支持平铺；
6. 支持CSS Sprite；
7. 支持Hover等伪类；

### 缺点
1. 额外加入了js文件（6.39k）和http请求，可以忽略不计；
2. 当文件载入之前，会先暂时呈现灰底
3. js文件过多的时候，可能会报错，导致js无法正常运行（这种情况极少出现，可以忽略不计）；



