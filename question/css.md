### 1.多行文本裁剪
案例：
多行文本随意行数裁剪

解决方法：  
  外层盒子上用下面的css代码
```css
.sihuanhang {
  // width: 200px; /** 可以加也可以去掉，table中建议去掉 **/
    word-break: break-all; /** 允许在单词内换行 **/
    text-overflow: ellipsis; /** 省略符号来代表被修剪的文本 **/
    display: -webkit-box; /** 盒子模型显示 **/
    -webkit-box-orient: vertical; /** 设置或检索伸缩盒对象的子元素的排列方式 **/
    -webkit-line-clamp: 2; /** 显示的行数 **/
    overflow: hidden;  /** 隐藏超出的内容 **/
  }
```
### 2.1px兼容问题
```css
.BorderTop,.BorderBottom,.BorderLeft,.BorderRight,.BorderAb { position: relative;}
.BorderTop:before,.BorderBottom:after {
  pointer-events: none;
  position: absolute;
  content: "";
  height: 1px;
  background: #e5e5e5;
  left: 0;
  right: 0;
}
.BorderTop:before {top: 0}
.BorderBottom:after {bottom: 0}
.BorderLeft:before,.BorderRight:after {
  pointer-events: none;
  position: absolute;
  content: "";
  width: 1px;
  background: #e5e5e5;
  top: 0;
  bottom: 0;
}
.BorderLeft:before {left: 0}
.BorderRight:after {right: 0}
.BorderAb:after {
  position: absolute;
  content: "";
  top: 0;
  left: 0;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  border: 1px solid #e5e5e5;
  pointer-events: none;
}

@media (-webkit-min-device-pixel-ratio:1.5),(min-device-pixel-ratio:1.5),(min-resolution: 144dpi),(min-resolution:1.5dppx) {
  .BorderTop:before,.BorderBottom:after {-webkit-transform:scaleY(.5);transform: scaleY(.5) }
  .BorderLeft:before,.BorderRight:after {-webkit-transform: scaleX(.5); transform: scaleX(.5) }
  .BorderAb:after { width: 200%; height: 200%;-webkit-transform: scale(.5); transform: scale(.5) }
  .BorderTop:before,.BorderLeft:before,.BorderAb:after {-webkit-transform-origin: 0 0;transform-origin: 0 0}
  .BorderBottom:after,.BorderRight:after { -webkit-transform-origin: 100% 100%;transform-origin: 100% 100%}
}
 
@media (-webkit-device-pixel-ratio:1.5) {
  .BorderTop:before,.BorderBottom:after { -webkit-transform: scaleY(.6666); transform: scaleY(.6666) }
  .BorderLeft:before,.BorderRight:after {-webkit-transform: scaleX(.6666); transform: scaleX(.6666)}
  .BorderAb:after {width: 150%; height: 150%;-webkit-transform: scale(.6666); transform: scale(.6666) }
}
 
@media (-webkit-device-pixel-ratio:3) {
  .BorderTop:before,.BorderBottom:after { -webkit-transform: scaleY(.3333); transform: scaleY(.3333)}
  .BorderLeft:before,.BorderRight:after { -webkit-transform: scaleX(.3333); transform: scaleX(.3333)}
  .BorderAb:after {width: 300%;height: 300%; -webkit-transform: scale(.3333);transform: scale(.3333)}
}
```
### 3.图片垂直居中

解决方法：  
  外层盒子上用下面的css代码
(1) background center
```css
  width: 500px;
  height: 500px;
  border: 1px solid red;
  background: url(img/redbb.png) center center no-repeat;
  background-size: 40px 40px;
```
(2) table-cell
```css
  width: 500px;
  height: 500px;
  border: 1px solid red;
  text-align: center;
  vertical-align: middle;
  display: table-cell;
```
(3)table-cell
```css
  .parent1 {
    display: table;
    height: 300px;
    width: 300px;
  }
  .parent1 .child {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
  }
```
### 4.清除浮动

解决方法：  
  外层盒子上用下面的css代码
(1) 添加空元素
(2) :after伪选择符 父级添加 .clearfix
```css
  .clearfix:after {
    content: "\0020";
    display: block;
    height: 0;
    clear: both;
  }
  .clearfix {
    zoom:1;
  }
```