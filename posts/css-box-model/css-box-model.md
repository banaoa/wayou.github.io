## CSS 盒模型

编程中我们常讲万物皆对象，面向对象编程嘛。写 CSS 的时候，则万物皆矩形，元素即方块。

![CSS 盒模型镇楼图](https://raw.githubusercontent.com/wayou/wayou.github.io/master/posts/css-box-model/assets/boxdim.png)

_CSS 盒模型镇楼图 - 图片来自 W3C_

页面中的元素，包含内容块（content），内边距（padding），边框（border）以及外边距（margin）四个区域。每个部分都有其自己的范围及边界。

### 四个边界线

上图中每条线代表了一种边界。由这些边界线定义出来的区域便是以上四个部分。

* 内容块（content）边界线  
指的是正中间包含长宽（[width](https://www.w3.org/TR/CSS2/visudet.html#Computing_widths_and_margins),[height](https://www.w3.org/TR/CSS2/visudet.html#Computing_heights_and_margins)）属性的内容盒子的边界线，也称作内边界线，其大小一般由该元素[渲染的内容](https://www.w3.org/TR/CSS2/conform.html#rendered-content)决定，四个方向的内边界线定义出了内容盒子。

* 内边距（padding）边界线  
包含内边距的边框，如果元素的内边距为0，那边这个边框就与内容块边界线重叠。

* 边框（border）边界线  
包含元素边框属性的边界线，如果元素的边框为0，那边这个边框就与内边距的边界线重叠。

* 外边距（margin）边界线  
包围着元素外边距区域的边界线，如果元素的外边界为0，那么这个边界线就与边框的边界线重叠。

以上每个边界线都可以细分为上下左右四个方向。

### 元素的大小

元素内容区域的大小，即元素的长宽，由许多因素决定：

* 元素是否设置了长宽属性
* 元素是否包含内容或者子元素
* 是否为表格元素
* ...

所以，关于元素长宽的问题单独在 [visual formatting model details](https://www.w3.org/TR/CSS2/visudet.html)有讨论。

### 背景色

除外边距始终为透明外，其他区域的背景色由元素的 `background` 属性来决定。


### 一个示例

拿 W3C 的示例代码来看看上述概念的具体体现。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HTML>
  <HEAD>
    <TITLE>Examples of margins, padding, and borders</TITLE>
    <STYLE type="text/css">
      UL { 
        background: yellow; 
        margin: 12px 12px 12px 12px;
        padding: 3px 3px 3px 3px;
                                     /* No borders set */
      }
      LI { 
        color: white;                /* text color is white */ 
        background: blue;            /* Content, padding will be blue */
        margin: 12px 12px 12px 12px;
        padding: 12px 0px 12px 12px; /* Note 0px padding right */
        list-style: none             /* no glyphs before a list item */
                                     /* No borders set */
      }
      LI.withborder {
        border-style: dashed;
        border-width: medium;        /* sets border width on all sides */
        border-color: lime;
      }
    </STYLE>
  </HEAD>
  <BODY>
    <UL>
      <LI>First element of list
      <LI class="withborder">Second element of list is
           a bit longer to illustrate wrapping.
    </UL>
  </BODY>
</HTML>
```

> 感觉这种大写的标签才是正规的 XML 语言该有的样子。

然后下面是它给出的效果图：

![CSS 盒模型示例](https://raw.githubusercontent.com/wayou/wayou.github.io/master/posts/css-box-model/assets/boxdimeg.png)

_CSS 盒模型示意_

我不想吐槽官方文档这个示例图了，显示严重偏色不说，`li` 之间的外边距塌缩（margin collapsing，这一概念后面会介绍）从图中硬是看不出来。

![CSS 盒模型示例](https://raw.githubusercontent.com/wayou/wayou.github.io/master/posts/css-box-model/assets/boxdimeg-fixed.png)

_CSS 盒模型示例_

### 外边距属性

外边距属性包含 `margin-top`,`margin-right`,`margin-bottom`,`margin-left` 以及 `margin`。其中 `margin` 是前四种的一种简写，即可以通过它一次性设置四个方向的外边距属性值。外边距属性适用于所有元素，除了非替代型行内元素。

*科普小贴士*
> 替代型元素（replaced elements）与非替代型元素（non-replaced elements）。前者指内容不由外部提供的元素，大部分 HTML 元素为这一类。后者是指元素内容由嵌入而来，譬如 img, applet, input, textarea, select 还有 object 等，为典型的替换型元素。

需要说明一下的是 `margin` 的值。

* 如果 `margin` 只有一个值，则四个方向都用这个值
* 如果 `margin` 只有两个值，则 `margin-top` `margin-bottom` 合用第一个值， `margin-right` `margin-left` 合用第二个值
* 如果 `margin` 只有三个值，则 `margin-top` 使用第一个值，`margin-right` 使用第二个值，`margin-bottom` 使用第三个值，`margin-left` 复制与之对应的 `margin-right` 的值  
```css
margin: 1em 2em 3em;

/*相当于：*/
margin-top: 1em;
margin-right: 2em;
margin-bottom: 3em;
margin-left: 2em;
```
* 如果 `margin` 只有四个值，则四个方向按上右下左的顺序对应取值

#### 外边距的塌缩/ Margin Collapsing

关于外边距，有个有趣的点需要提一下，即外边距的塌缩（margin collapsing）。

毗邻元素或者父子元素间，垂直方向的外边距如果没有边框，内边距等隔开，则两者的外边距会相遇，此时会发生塌缩，合二为一。

下面是个很好的例子，展示了嵌套情况下，垂直方向相遇后塌缩成一个外边距的情况，非常的明显。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        .box {
            margin: 10px;
        }

        .a {
            background: #777;
        }

        .b {
            background: #999;
        }

        .c {
            background: #bbb;
        }

        .d {
            background: #ddd;
        }

        .e {
            background: #fff;
        }
    </style>
</head>

<body>
    <div class="box a">
        <div class="box b">
            <div class="box c">
                <div class="box d">
                    <div class="box e">
                        这里嵌套的 div 元素，每个都有 10px 的外边距，且都有不同的背景色，他们垂直方向的外边距会因为 margin collapsing 而塌缩成一个，水平方向的外边距则不会。
                    </div>
                </div>
            </div>
        </div>
    </div>
    </div>
</body>
</html>
```

![外边距的塌缩](https://raw.githubusercontent.com/wayou/wayou.github.io/master/posts/css-box-model/assets/margin-collapsing.png)

_外边距的塌缩_

从上面提到的条件来看，之所以发生塌缩，是因为两个外边距直接相遇，而中间没有阻拦。如果想防止塌缩，那么思路就很简单了，为每个元素设置边框，或者内边距都可以。

```css
.box {
    margin: 10px;
    border: 1px solid;
}
```

![使用边框修复外边距的塌缩](https://raw.githubusercontent.com/wayou/wayou.github.io/master/posts/css-box-model/assets/margin-collapsing-fixed-with-border.png)

_使用边框修复外边距的塌缩_

另外，绝对或相对定位，发生浮动这些元素，不会发生外边距的塌缩。

两个外边距发生塌缩的必要条件是：

* 都属于未脱离文档流（in flow）的[块级元素](https://www.w3.org/TR/CSS2/visuren.html#block-boxes)，且在同[一块级格式上下文](https://www.w3.org/TR/CSS2/visuren.html#block-formatting)。
* 中间没有行级盒子（line box），浮动清除（clear），内边距（padding）或者边框（border）隔开
* 两者都属于垂直方向且满足毗邻的条件，即无论是父子包含关系或者兄弟毗邻关系，需要两者直接相遇的情况
  - 父容器上外边距与子元素的上外边距
  - 元素下外边距与毗邻的下一个兄妹元素的上外边距
  - 父容器下外边距与子元素下外边距
  - 元素本身的上外边距与下外边距，在自身高度为0的时候，即空元素


#### 塌缩时合并后的外边距计算规则 

* 正常情况两个外边距合并时取值较大的那个
* 如果其中一个是负值，则合并后的外边距为两者相加的结果
* 如果两者都是负值则取绝对值较大的

*科普小贴士*
> in flow 与 out of flow
> 根节点元素（前端页面中一般为 html）, 绝对及固定定位的元素，属于脱离文档流的情况，叫作 out of flow。没有脱离文档流的元素叫作 in flow。

> [行级盒子（line box）](https://stackoverflow.com/a/32022060/1553656)。 指的是一行内的矩形区域。如果多个行内元素无法在一行显示，则会形成多行，每一行是一个 line box。最好的例子是段落（p 标签），多行情况下它就是一个垂直方向上叠加的多个行级盒子。

### 内边距属性

与外边距不同，它的值不能为负数。其他与外边距类似，`padding` 是四个方向的缩略形式。其接收到的值与四个方向的映射关系与外边距的规则是一致的。

内边距区域的颜色由元素的 `background` 属性指定。

### 边框属性

边框属性包括宽度（width）, 颜色（color）及样式（style）。

### border-width

`border-width`指定的是边框的宽度。除了常规的值外，还有预设三个语义化的值 `thin`，`medium` 和 `thick`，具体的效果各浏览器的实现会有差异。

同样分为四个方向上的分值：

* border-top-width
* border-right-width
* border-bottom-width
* border-left-width

因为 `border-width` 是上述四个分量的缩写，可以通过 `border-width` 一次性指定。接收到值的个数与四个方向上的映射规则同外边距属性。

### border-color

同样，`border-color` 也是分为四个方向上的分量值，可以通过 `border-color` 这一简写形式一次性指定。

当元素没有显式指定时，其值沿用元素的 `color` 值。

```css
p { 
  color: red; 
  background: white; 
  border: solid;
}
```

上面 `p` 标签的边框颜色将会是红色。


### border-style

边框的样式，它的值有如下几种：

* none - 无边框，此时边框宽度为0。在有冲突的值被设置的情况下，它有最低的优先级。
* hidden - 同 `none`，但在冲突情况下有最高优先级。
* dotted - 由圆点排列围绕而成的边。
* dashed - 虚线样式。
* solid - 实线。
* double - 双实线，两条线及之间的间距加起来的宽度为 border-wdith。
* groove - 雕刻样式，凹陷的样式，与 ridge 相反。
* ridge - 浮雕样式，突起状，与 groove 相反。
* inset - 内边线样式。
* outset - 外边线样式。

由于 `border-style` 初始值是 `none`，所以没有显式指定元素的 `border-style` 时，是看不见边框的。

对于以上的 `border-color`，`border-width` 及 `border-style`，可以通过 `border` 属性来一次性指定。

```
border: <br-width> || <br-style> || <color>
```

与 `margin`，`padding`不同的是，`border` 是一次性指定四个方向的值，如果需要设置某个方向的值，只能单独使用该方向的边框属性来设置。


### 相关资源

* [Box model](https://www.w3.org/TR/CSS2/box.html#img-boxdim)
* [Visual formatting model](https://www.w3.org/TR/CSS2/visuren.html)
* [what is a non-replaced inline element?](https://stackoverflow.com/questions/12468176/what-is-a-non-replaced-inline-element)
* [Block formatting context](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)
* [In the CSS Visual Formatting Model, what does “the flow of an element” mean?](https://stackoverflow.com/a/40325328/1553656)
