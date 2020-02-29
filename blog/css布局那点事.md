
> 说到布局，一定离不了自身的尺寸、文档流、相邻元素、父元素等。目的是让元素显示在应该出现的位置（好像是废话），然后其他地方最好也能复用，如果不同分辨率上体验一致就更好了

针对上面这个目的，接下来我们就开始讲讲怎么让元素显示在应该在的位置，开始正题：

<h1 style='text-align:center'>理论部分</h1>

------

## 需要知道的基础知识

**以下知识点需要有所了解**

### 默认样式
浏览器对所有标签都有默认样式，比如文字默认是黑色，div默认是 <code>block</code>(块)元素，span默认是 <code>inline</code> (行内)元素等

### 属性继承
有些属性可以继承，有些属性不能继承，如<code>color</code>可以继承，<code>width</code>就不能，具体哪些可以继承，哪些不能，一个简单方式记忆就是跟**文字相关属性大都可以继承，跟布局相关属性大都继承**
- **常见的可继承属性**：color,font(复合属性),letter-spacing,word-spacing...
- **不可继承属性**：display、margin、border、padding、background、height、min-height、max-height、width、min-width、max-width、overflow、position、left、right、top、bottom、z-index、float、clear、table-layout、vertical-align...

### 单一属性与复合属性
属性分为单一属性和复合属性，单一属性如 <code>color:red</code> ，复合属性如（以下三中表示等价）:
```css
padding: 10px 20px;
```
```css
padding: 10px 20px 10px 20px;
```
```css
padding-top:10px;
padding-right:20px;
padding-bottom:10px;
padding-left:20px;
```

<!-- more -->

### CSS单位

#### 尺寸单位
常见的尺寸单位有 <code>px</code>、<code>em</code>、<code>rem</code>、<code>%</code>、<code>pt</code>等等，初学者先记住<code>px</code>和<code>%</code>

#### 颜色
常见颜色表示有以下几种：
- 字符串表示如 <code>color:red</code>，顾名思义，红色
- rgb表示如 <code>color:rgb(0,0,255)</code>，表示red:0, green:0, blue: 255,即纯蓝色
- rgba表示如 <code>color:rgb(0,0,255, 0.5)</code>，比rgb多了一个appha值,即 0至1 的透明度
- 16进制(HEX)表示如 <code>color:#ff0000</code>，如果每连续两个二进制相同，可以简写成一个，如果左边可以简写成<code>color:#f00</code>，字母不区分大小写
- hsl颜色(不常用)，如<code>color:hsl(120, 50%, 50%)</code>，
  - 说明：hsl是hue(色调)、saturation(饱和度)、lightness(亮度)首字母缩写，是颜色的另一表示
  - 取值：
    - **hue**为 0(红色)-360(红色)，其实是色环的360度，当然也可以为380，其实与 20相同效果；
    - **sturation**为 0%(全灰)-100%(全彩)；
    - **lightness**为 0%(黑色)-100%(白色)；
- hsla颜色(不常用)，如<code>color:hsl(120, 50%, 50%, 0.5)</code>

关于颜色之间的转化可以进入 **[传送门](https://color.adobe.com/zh/create/color-wheel)**

-------

## 与布局相关的基本属性

- **display**
- **width, height**
- **max-width, max-height, min-width, min-height**
- **padding, border, margin**
- **float, overflow， clear**
- **position, z-index, top, right, bottom, left**

这些都是比较常用的属性，下面逐个做下介绍，如果你对 **css盒模型** 不了解的话推荐看下之前的一篇文章，**[传送门](http://tt-ghost.github.io/2016/05/22/The-ABC-of-front-end)**


### 第一组：display

#### 用途
设置元素的显示属性，

#### 取值
常见取值如下
> 
**none**(隐藏), 常用于用户与页面交互时显示、隐藏元素，隐藏之后的元素，再设置别的属性都不会生效
**inline**(行内元素)，设置width和height会被忽略, 会随正常文档流从左到右，从上到下布局
**block**(块元素)，可以设置width, height
**inline-block**(行内块元素)，兼有inline和block属性值的特点，既能设置width、height，也会随正常文档流左右布局

还有很多其他属性值，如 **flex**、**table**、**table-cell**、**table-row**、**table-column**等，其中flex是css3新推出的属性，用法较多，本文暂不介绍。其他属性值使用相对较少

### 第二组: width, height
设置元素content宽度和高度。如果用百分比如<code>width: 30%</code>则表示为父容器content部分宽度的30%

### 第三组: max-width, max-height, min-width, min-height
设置元素最大、最小宽高，主要不同分辨率浏览器适配使用，经常媒体查询(@media)一起使用，如：
```css
@media (max-width:1000px){
  body: 90%;
}
```
表示浏览器宽度小于或等于1000px时， body 宽度为父容器的 90%

### 第四组: padding, border, margin
设置元素的内边距、边框、外边距，这里需要理解盒模型，**[传送门](http://tt-ghost.github.io/2016/05/22/The-ABC-of-front-end)**，这里也不做解释了

### 第五组: float, clear, overflow
这一组与下一组是难点也是重点。

#### float属性
用于指定元素是否应该浮动或怎样浮动，取值如下：
> 
**none**: 不浮动，也是所有元素的默认值，
**left**: 左浮动，值为left或right时，元素就脱离之前的文档流，其排版只受其他元素(也有float属性且非none的元素)影响。其兄弟元素或父元素如果没有float属性，就会当这个元素不存在，占其位置。所以一旦有了float属性就好像长了翅膀，飞起来后就不跟其他小伙伴玩了，其他小伙伴当然会占用ta之前的位置
**right**: 右浮动，同上

浮动元素有以下特点：
> 
元素本身会塌陷：如果float之前没有设置width、height或为auto，则元素塌陷后的width、height为内部元素整体占的宽高，其本身会当作类似 **inline-block** 的元素影响周围元素

听起来有点绕，看下面代码
```html
<!doctype html>
<html>
  <body>
    <div class='container'>
      <div class='sub-1'>
        <div class='sub-2'>sub-2</div>
      </div>
      <span class='text-1'>text-1</span>
    </div>
    <span class='text-2'>text-2</span>
  </body>
<html>
```

```css
// 之前
.sub-1{}
.sub-2{height:10px;width:100px;}
```
```css
// 之后
.sub-1{float:left;}
.sub-2{height:10px;width:100px;}
```
第一个样式显示的效果是，三行文本分三行显示，第二个样式显示效果是 **sub-2**与**text-1**显示在了一行，而**text-2**显示在了第二行
> 
第一个样式：显示三行应该不需要解释，**sub-2**被块元素 <code>.sub-1</code> 包裹，其他同理;**.sub-1**的与**.sub-2**相同，宽度为通栏
第二个样式：给 **.sub-1** 加入float后,其宽高都与**.sub-2**相同，并且是脱离文档流状态，类似与**inline-block**显示特性，所以**text-1**会在其后排版，因为**.container**没有脱离文档流，所以**.text-2**会换行显示

留下一个疑问：假如**.sub-2**高度为**30px**时会发生什么？欢迎互动

#### clear属性
清除浮动，主要将有 **clear** 属性且值为**left**或**right**或**both**的元素在文档中以前的有浮动情况清除掉，使后面的文档不受float干扰，有以下取值
> 
**none**:不清除浮动
**left**:清除左浮动
**right**:清除右浮动
**both**:清除左右浮动，比较常用

#### overflow属性
规定当容器内容溢出时如何显示，常见取值如下：
> 
**visible**:溢出内容会显示
**hidden**:溢出内容会隐藏
**scroll**:无论内容是否溢出都会出现滚动条
**auto**:如果内容溢出会出现滚动条，不溢出则没有


### 第六组: position, z-index, top, right, bottom, left
这一组最难的一个属性是position，理解当前元素相对谁定位后，其对应的**top**、**right**、**bottom**、**left**就很好理解了

#### position属性
CSS布局中，position几乎算是核心中的核心，因为你可以让人**任意元素在浏览器的任何位置**，也可以让元素**影响正常文档流**，也可以**不影响正常文档流**，而且还可以**限定子元素定位的相对父亲**，所以这么厉害的大招一定要学好，下面一个一个技能来学习，先看下取值：
> - **absolute**:绝对定位，一旦为此值，元素就会从直接父级容器开始找，如果父级元素有设置**position**属性，且不为**static**，则就把父级容器当作相对容器，然后停止继续向上查找，如果直接父级没有这个属性，则继续向上，找父级的父级元素，直到满足刚才的条件才停止查找，如果到最外层(html)都没有找到，则相对浏览器窗口，可见找爹的意志之坚定啊！

> - **relative**:相对定位，相对自身的位置，不会脱离文档流，如果同时设置了**top:10px**，此元素会离之前的位置向下移动 **10px** 距离，同时对除自身及内部元素以外的元素没有任何影响。

> - **fixed**:绝对定位，只相对浏览器窗口定位，配合**top**、**right**、**bottom**、**left**可实现指哪定哪的特技

> - **static**:静态定位，所有元素的默认定位方式

#### z-index属性
用在设置有**position**属性，且值不为**static**的元素的显示层级，比如通过**fixed**定位的两个元素都在浏览器左上角，到底谁在上面，谁在下面？这个问题就只能交给**z-index**了
> 取值为负整数、0、正整数，默认值为 -1

#### top, right, bottom, left
理解**position**定位后，找到相对元素，设置对应**top**、**right**、**bottom**、**left**，注意定位 **static**的元素不会生效

-------

## 文档流
个人觉得文档流通常有以下特点，可能不够准确
> 普通文档流就是 **inline** 及 **inline-block** 类型的元素从左到右、从上到下排版，相邻元素会互相影响，父元素会作为容器影响子元素
> 非普通文档流跟普通文档流对应，通常元素设置了**position**或**float**属性，使其尺寸大小及位置对兄弟元素不产生直接影响

-------

<h1 style='text-align:center;margin-bottom:50px'>实战部分</h1>

> 这里仅列出比较常见的布局方式，还有很多其他方式等待你去实现

如果对布局不太熟悉的话，建议每个列子都亲自写一遍，加深记忆

## 两栏布局
左侧定宽，右侧自适应

### 方案一 float
**.sidebar-right** 的 **margin-right** 也可以改成 **padding-left**
```html
<!doctype html>
<html>
  <head>
    <style>
      .sidebar-left{
        width:100px;
        float:left;
      }
      .sidebar-right{
        margin-left:100px;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='sidebar-left'>left</div>
      <div class='sidebar-right'>right</div>
    </div>
  </body>
<html>
```
### 方案二 position
一个 absolute，脱离文档流后，right元素只要设置个左边距即可
```html
<!doctype html>
<html>
  <head>
    <style>
      .container{
        position:relative;
      }
      .sidebar-left{
        width:100px;
        position:absolute;
        left:0;
      }
      .sidebar-right{
        margin-left:100px;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='sidebar-left'>left</div>
      <div class='sidebar-right'>right</div>
    </div>
  </body>
<html>
```
### 方案三 table-cell
这个例子中 **.container** 可以不设置，但是推荐写上，通常 **table-cell** 与父级 **table** 对应
```html
<!doctype html>
<html>
  <head>
    <style>
      .container{
        width:100%;
        display:table;
      }
      .sidebar-left, .sidebar-right{
        display: table-cell;
      }
      .sidebar-left{
        width:100px;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='sidebar-left'>left</div>
      <div class='sidebar-right'>right</div>
    </div>
  </body>
<html>
```


## 三栏布局

### 方案一 float
注意 .content 在DOM中的位置
```html
<!doctype html>
<html>
  <head>
    <style>
      .sidebar-left{
        float:left;
        width:100px;
      }
      .content{
        margin:0 100px;
      }
      .sidebar-right{
        float:right;
        width:100px;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='sidebar-left'>left</div>
      <div class='sidebar-right'>right</div>
      <div class='content'>content</div>
    </div>
  </body>
<html>
```

### 方案二 position
left及right元素设置了 absolute
```html
<!doctype html>
<html>
  <head>
    <style>
      .container{
        position:relative;
      }
      .sidebar-left{
        position:absolute;
        left:0;
        width:100px;
      }
      .content{
        margin:0 100px;
      }
      .sidebar-right{
        position:absolute;
        right:0;
        width:100px;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='sidebar-left'>left</div>
      <div class='content'>content</div>
      <div class='sidebar-right'>right</div>
    </div>
  </body>
<html>
```

### 方案三 table-cell
注意 **.content** 没有设置任何属性
```html
<!doctype html>
<html>
  <head>
    <style>
      .container{
        width:100%;
        display:table;
      }
      .sidebar-left, .sidebar-right{
        display: table-cell;
        width:100px;
      }
      .sidebar-right{
        width:100px;
        display: table-cell;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='sidebar-left'>left</div>
      <div class='content'>content</div>
      <div class='sidebar-right'>right</div>
    </div>
  </body>
<html>
```

-------

## 定位布局

### relative 相对定位
注意相对哪个元素定位
```html
<!doctype html>
<html>
  <head>
    <style>
      .box-1,.box-3{
        background-color: #eee;
      }
      .box-2{
        width:300px;
        position: relative;
        top:-30px;
        left:130px;
        background-color: #aaa;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='box-1'>
        轻轻的我走了， <br>
        正如我轻轻的来；<br> 
        我轻轻的招手， <br>
        作别西天的云彩。
      </div>
      <div class='box-2'>
        position:relative;<br>
        top: -30px;<br>
        left:130px
      </div>
      <div class='box-3'>
        那河畔的金柳，<br> 
        是夕阳中的新娘；<br> 
        波光里的艳影， <br>
        在我的心头荡漾。 
      </div>
    </div>
  </body>
<html>
```

### absolute 绝对定位
注意相对的父元素是 **.container** 而非 **body**
```html
<!doctype html>
<html>
  <head>
    <style>
      .container{
        position:relative;
        margin-left:200px;
      }
      .box-1,.box-3{
        background-color: #eee;
      }
      .box-2{
        width:300px;
        position: absolute;
        top:150px;
        left:30px;
        background-color: #aaa;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='box-1'>
        轻轻的我走了， <br>
        正如我轻轻的来；<br> 
        我轻轻的招手， <br>
        作别西天的云彩。
      </div>
      <div class='box-2'>
        position:absolute;<br>
        top: 150px;<br>
        left: 30px
      </div>
      <div class='box-3'>
        那河畔的金柳，<br> 
        是夕阳中的新娘；<br> 
        波光里的艳影， <br>
        在我的心头荡漾。 
      </div>
    </div>
  </body>
<html>
```

### fixed 绝对定位
上下滚动浏览器滚动条，观察灰色div位置
```html
<!doctype html>
<html>
  <head>
    <style>
      .container{
        position:relative;
        margin-left:200px;
      }
      .box-1{
        height:1000px;
      }
      .box-2{
        width:300px;
        position: fixed;
        top:100px;
        left:50%;
        background-color: #eee;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='box-1'>
        再别康桥 <br><br>
        作者: 徐志摩 <br><br>
        轻轻的我走了， <br>
        正如我轻轻的来； <br>
        我轻轻的招手， <br>
        作别西天的云彩。 <br>
          <br>
        那河畔的金柳， <br>
        是夕阳中的新娘； <br>
        波光里的艳影， <br>
        在我的心头荡漾。 <br>
          <br>
        软泥上的青荇， <br>
        油油的在水底招摇； <br>
        在康河的柔波里， <br>
        我甘心做一条水草！ <br>
          <br>
        那榆荫下的一潭， <br>
        不是清泉， <br>
        是天上虹； <br>
        揉碎在浮藻间， <br>
        沉淀着彩虹似的梦。 <br>
          <br>
        寻梦？撑一支长篙， <br>
        向青草更青处漫溯； <br>
        满载一船星辉， <br>
        在星辉斑斓里放歌。 <br>
          <br>
        但我不能放歌， <br>
        悄悄是别离的笙箫； <br>
        夏虫也为我沉默， <br>
        沉默是今晚的康桥！ <br>
          <br>
        悄悄的我走了， <br>
        正如我悄悄的来； <br>
        我挥一挥衣袖， <br>
        不带走一片云彩。
      </div>
      <div class='box-2'>
        position: fixed;<br>
        top:100px;<br>
        left:50%;
      </div>
    </div>
  </body>
<html>
```

-------

## 响应式布局

### max-width 最大宽
拖动浏览窗口，查看灰色背景div宽度变化
```html
<!doctype html>
<html>
  <head>
    <style>
      .box-1{
        width:50%;
        max-width:400px;
        background-color:#ccc;
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='box-1'>
        轻轻的我走了， <br>
        正如我轻轻的来； <br>
        我轻轻的招手， <br>
        作别西天的云彩。 <br>
      </div>
    </div>
  </body>
<html>
```

### media 媒体查询
常用语适配PC和移动端站点，拖动浏览器改变窗口大小试试，窗口小于 **767px** 时，观察 **.box-1**、**.box-2** 的位置
```html
<!doctype html>
<html>
  <head>
    <style>
      .box-1,.box-2{
        width:50%;
        float:left;
      }
      @media (max-width:767px){
        .box-1,.box-2{
          width:100%;
          float:none;
        }
      }
      </style>
  </head>
  <body>
    <div class='container'>
      <div class='box-1'>
        轻轻的我走了， <br>
        正如我轻轻的来； <br>
        我轻轻的招手， <br>
        作别西天的云彩。 
      </div>
      <div class='box-2'>
        那河畔的金柳， <br>
        是夕阳中的新娘； <br>
        波光里的艳影， <br>
        在我的心头荡漾。
      </div>
    </div>
  </body>
<html>
```

-------