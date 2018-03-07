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