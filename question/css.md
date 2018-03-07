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