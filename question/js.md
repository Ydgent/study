### 1.利用FormData提交文件、图片
案例：
上传图片、文件（不依赖后台接口）

解决方法：（核心代码）

（1）文件上传

```javascript
<FormItem
    label="JSON格式配置数据："
 >
    <input ref="file" type="file" style={{display: 'none'}} onChange={ e => this.setState({file: e.target.files[0]}) }/>
    <Button type="ghost" onClick={ () => {
          this.refs.file.click();
    }}>
      <Icon type="upload" /> 点击上传
    </Button>
    <span style={{marginLeft: 15}}>{file && file.name}</span>
 </FormItem>
```
```javascript
const formData = new FormData();
formData.append('file', this.state.file);
```

（2）图片上传
```css
<FormItem
    label="图片上传："
 >
 <div className="icon-config-pic-card">
    {!fileUrl && <Input type="file" onChange={e => this.fileChange(e.target.files[0])}/>}
    {
      fileUrl &&
      <div style={{position: 'relative', height: '100%'}}>
        <img src={fileUrl} style={{ width: '100%', height: '100%', display: 'block'}}/>
        <div style={{position: 'absolute', bottom: 0, left: 0, right: 0, background: 'rgba(0,0,0,0.5)', color: '#fff', textAlign: 'left' }}>
          <div onClick={this.showModal}>预览</div>
          {type !== 'check' && <div onClick={this.clear} style={{padding: '0 10px', float: 'right'}}>删除</div>}
        </div>
      </div>
    }
    <Icon type="plus" style={{fontSize: '28px', marginTop: 20}}/>
    <div style={{color: '#999'}}>上传图片</div>
 </div>
</FormItem>
css部分
.icon-config-pic-card{
  border: 1px dashed #d9d9d9;
  width: 96px;
  height: 96px;
  border-radius: 4px;
  background-color: #fbfbfb;
  text-align: center;
  cursor: pointer;
  -webkit-transition: border-color 0.3s ease;
  transition: border-color 0.3s ease;
  display: inline-block;
  vertical-align: top;
  margin-right: 8px;
  margin-bottom: 8px;
  overflow: hidden;
  // padding:20px;
  &:hover{
    border-color: #108ee9;
  }
  & .ant-input{
  height: 100%;
  width: 100%;
  margin-bottom: -100%;
  opacity: 0;
  cursor: pointer;
  }
}
```
```javascript
const createURL = input => {
  return window.URL.createObjectURL && window.URL.createObjectURL(input) || input;
};
const revokeURL = url => {
  if (window.URL.revokeObjectURL) {
    window.URL.revokeObjectURL(url);
  }
};
fileChange = (e) => {
    const file = e;
    this.setState({
      fileUrl: (typeof file === 'string') ? file : createURL(file),
    });
}
clear = () => {
    revokeURL(this.state.fileUrl);
}

提交：
const formData = new FormData();
formData.append('FILTER_LOGO', values.file)
```
### 2. 文本复制
案例：
实现某文本的复制，可以通过control + v获取。

解决方法：（核心代码）
js

```
copyLink = (j) => {
  this.refs.hide2.value = j;
  this.refs.hide2.select();
  document.execCommand('Copy'); // 执行浏览器复制命令
  message.success('复制成功');
}
```
html

```
<Button type="primary" size="small" onClick={this.copyLink.bind(this, j)}>复制</Button>
<input type="text" ref="hide2" style={{ width: 0.2, border: 'none', height: 0.2 }}/>
```
### 3.页面加载回滚到顶部
案例：
页面过长，在通过路由跳转时会保留原来浏览位置，对使用者不友好。

解决方法：（核心代码）
js

```
// 页面加载时回到顶部
window.onhashchange = () => {
  document.body.scrollTop = document.documentElement.scrollTop = 0;
};
```
### 4.一些用到的典型的正则
案例：正则匹配字符串，实现某些功能。

例子：
```javascript
// 转化号码
phone.replace(/(\d{3})\d{4}(\d{4})/, '$1****$2') // "183****5136"
// 解析url
const url = /(\w+):\/\/([\w.]+)\/(\S*)/;
const text = "Visit my github http://github.com/Ydgent";
text.match(url); // ["http://github.com/Ydgent", "http", "github.com", "Ydgent", index: 16, input: "Visit my github http://github.com/Ydgent"]
```
推荐地址：
https://c.runoob.com/front-end/854 

http://www.lovebxm.com/2017/05/31/RegExp/
### 5.鼠标滚轮事件与事件绑定函数
案例：
标签添加滚轮时间兼容ie

解决方法：（核心代码）

```javascript
  var oDiv = document.getElementById('box');
  // 鼠标滚轮事件
  function onMouseWheel(ev){
    var oEvent=ev||event;
    var bDown=true;
    bDown=oEvent.wheelDelta?oEvent.wheelDelta<0:oEvent.detail>0;
    if(bDown){   //滚轮向下滚动
      console.log('向下滚动')
    } else{
      console.log('向上滚动')
    }
    if(oEvent.preventDefault){  //FF下addEventListener用preventDefault阻止莫认事件；
      oEvent.preventDefault();
    }
    return false;
  }

  // 事件绑定
  // element.addEventListener(event, function, useCapture)
  // element.attachEvent(event, function) // ie下需要加on
  function addEventHandler (target,type,fn){
    if(target.attachEvent){ 
      target.attachEvent('on'+type,fn);
    } else{ 
      target.addEventListener(type,fn,false);
    }
  }
  // 事件移除
  function removeEventHandler(target,type,fn){
    if(target.removeEventListener){
    target.removeEventListener(type,fn);
    }else{
    target.detachEvent("on"+type,fn);
    }
  }
  addEventHandler(oDiv,'mousewheel',onMouseWheel);    //addEventHandler为自定义函数。事件绑定在上面;
  addEventHandler(oDiv,'DOMMouseScroll',onMouseWheel);
```
### 6.深拷贝
案例：
对象深拷贝，防止干扰。

解决方法：（核心代码）
```javascript
  // 第一种方法
  function deepCopy(p, c) {
    var c = c || {};
    for (var i in p) {
      if (typeof p[i] === 'object') {
        c[i] = (p[i].constructor === Array) ? [] : {};
        deepCopy(p[i], c[i]);
      } else {
        c[i] = p[i];
      }
    }
    return c;
  }
  var Doctor = deepCopy(Chinese);
  Chinese.birthPlaces = ['北京','上海','香港'];
  Doctor.birthPlaces.push('厦门');
  alert(Doctor.birthPlaces); //北京, 上海, 香港, 厦门
  alert(Chinese.birthPlaces); //北京, 上海, 香港
  // 第二种方法
  JSON.parse(JSON.stringify);

```
### 6.判断机型（ios or android)

解决方法：（核心代码）
```javascript
  // 第一种方法
  var ua = navigator.userAgent.toLowerCase();
  if (/iphone/.test(ua)) {
    // ios
  }else if (/android/.test(ua)) {
    // android
  }

  // 第二种方法
  var u = navigator.userAgent;
  var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1; //android终端
  var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
  if (isAndroid) {
    //android终端  showToast android的原生方法
    android.showToast("哈哈啊哈");
    alert('android');

    var getMsg = "{" + '"share_txt":' + '"' + share_txt + '"' + ',"title_txt":' + '"' + share_txt + '" ' + ', "webHref":' + '"' + ApiRoot + "/pages/account/share_loginregister.html?inviter=" + (investorId != null ? investorId : 0) + '"' + ',"imgHref":' + '"' + MarketRoot + "/Content/img/LOGO_300x300.png" + '"' + "}";
      // inviteFriends是android原生方法，将getMsg传给android
      window.android.inviteFriends(getMsg);
  }
  if (isiOS) {
    //ios终端
    alert('ios')
  }
```
### 7.rem布局

解决方法：（核心代码）
```javascript
  (function (doc, win) {
    var docEl = doc.documentElement,
      resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
      recalc = function () {
        var clientWidth = docEl.clientWidth;
        if (!clientWidth) return;
        docEl.style.fontSize = 100 * (clientWidth / 320) + 'px';
      };

    // Abort if browser does not support addEventListener
    if (!doc.addEventListener) return;
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
  })(document, window);
```
### 8.屏幕旋转的事件和样式

解决方法：（核心代码）
```javascript
  function orientInit(){
    var orientChk = document.documentElement.clientWidth >   document.documentElement.clientHeight?'landscape':'portrait';
    if(orientChk =='lapdscape'){
        //这里是横屏下需要执行的事件
    }else{
        //这里是竖屏下需要执行的事件
    }
  }
  orientInit();
  window.addEventListener('onorientationchange' in window?'orientationchange':'resize', function(){
      setTimeout(orientInit, 100);
  },false)

  //CSS处理
  //竖屏时样式
  @media all and (orientation:portrait){   }
  //横屏时样式
  @media all and (orientation:landscape){   }
```
参考地址：http://blog.163.com/lzyi_117/blog/static/117925906201721111141438/（软键盘与定位）
















