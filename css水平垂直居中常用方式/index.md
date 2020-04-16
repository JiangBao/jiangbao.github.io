# CSS水平垂直居中常用方式

![css居中](https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/CSS%E5%B1%85%E4%B8%AD.png)

## 水平居中
### 1. 行内元素
```html
<body>
  <div class="center-wrap">水平居中</div>
</body>
```
```css
.center-wrap {
  text-align: center;
}
```

### 2. 确定宽度块级元素
```html
<body>
  <div class="center-wrap">水平居中</div>
</body>
```
```css
.center-wrap {
  border: 1px solid;
  width: 600px;
  margin: 0 auto;
}
```

### 3. 未知宽度块级元素
#### table
`display: table`配合`margin: 0 auto`
```html
<body>
  <div class="center-wrap">水平居中</div>
</body>
```
```css
.center-wrap {
  border: 1px solid;
  display: table;
  margin: 0 auto;
}
```

#### inline-block(多个块状元素)
子元素设置`display: inline-block`，父元素设置`text-align: center`
```html
<body>
  <div class="center-wrap">
    <div class="child-wrap">元素1</div>
    <div class="child-wrap">元素2</div>
  </div>
</body>
```
```css
.center-wrap {
  text-align: center;
}

.child-wrap {
  border: 1px solid;
  display: inline-block;
}
```

#### flex
```html
<body>
  <div class="center-wrap">
    <div class="child-wrap">元素1</div>
    <div class="child-wrap">元素2</div>
  </div>
</body>
```
```css
.center-wrap {
  display: flex;
  justify-content: center;
}

.child-wrap {
  border: 1px solid;
}
```

#### 绝对定位+transform
```html
<body>
  <div class="center-wrap">
    <div class="child-wrap">居中元素</div>
  </div>
</body>
```
```css
.center-wrap {
  border: 1px solid;
  position: relative;
  width: 600px;
  height: 600px;
}

.child-wrap {
  border: 1px solid;
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
}
```


## 垂直居中
### 1. 文本类
#### 单行文本
* 设置padding-top=padding-bottom，利用padding撑起
* line-height=height
```html
<body>
  <p class="vertical-center">单行文本垂直居中</p>
</body>
```
```css
.vertical-center {
  border: 1px solid;

  height: 60px;
  line-height: 60px;

  padding: 30px 0;
}
```

#### 多行文本
```html
<body>
  <div class="center-wrap">
    <div class="text-area">
      <p>多行垂直居中文本</p>
      <p>多行垂直居中文本</p>
      <p>多行垂直居中文本</p>
    </div>
  </div>
</body>
```
```css
.center-wrap {
  border: 1px solid;
  width: 600px;
  height: 600px;
  display: table;
}

.text-area {
  display: table-cell;
  vertical-align: middle;
}
```

### 2. 块级元素
#### position+margin
父元素设置*相对定位*，子元素设置*绝对定位*，top、bottom设置为0，margin: auto实现自适应居中
```html
<body>
  <div class="center-wrap">
    <div class="center-div">这是垂直居中元素</div>
  </div>
</body>
```
```css
.center-wrap {
  border: 1px solid;
  position: relative;
  width: 600px;
  height: 600px;
}

.center-div {
  border: 1px solid;
  position: absolute;
  width: 300px;
  height: 200px;
  top: 0;
  bottom: 0;
  margin: auto;
}
```

#### flex
两种方法，需要知道父元素高度
* 父元素设置display: flex; align-items: center
* 父元素设置display: flex; 子元素设置margin: auto实现自适应居中
```html
<div class="center-wrap">
  <div class="center-div">这是垂直居中元素</div>
</div>
```
```css
.center-wrap {
  border: 1px solid;
  height: 600px;
  display: flex;
  align-items: center;
}
```
```css
.center-wrap {
  border: 1px solid;
  height: 600px;
  display: flex;
}

.center-div {
  border: 1px solid;
  margin: auto;
}
```

#### transform
父元素相对定位，子元素绝对定位，设置transform
```html
<div class="center-wrap">
  <div class="center-div">这是垂直居中元素</div>
</div>
```
```css
.center-wrap {
  border: 1px solid;
  height: 600px;
  position: relative;
}

.center-div {
  border: 1px solid;
  width: 200px;
  height: 200px;
  position: absolute;
  top: 50%;
  transform: translate(0, -50%);
}
```

## 水平垂直居中实践
对于以下场景，要使[class=center-div]的元素水平垂直居中，自己常用的水平垂直同时居中的方法
```html
<div class="center-wrap">
  <div class="center-div">这是居中元素</div>
</div>
```

### flex
```css
.center-wrap {
  border: 1px solid;
  width: 600px;
  height: 600px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.center-div {
  border: 1px solid;
  width: 200px;
  height: 200px;
}
```

### 绝对定位+margin:auto
```css
.center-wrap {
  border: 1px solid;
  width: 600px;
  height: 600px;
  position: relative;
}

.center-div {
  border: 1px solid;
  width: 200px;
  height: 200px;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
}
```

### 绝对定位+transform
```css
.center-wrap {
  border: 1px solid;
  width: 600px;
  height: 600px;
  position: relative;
}

.center-div {
  border: 1px solid;
  width: 200px;
  height: 200px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

### table-cell
```css
.center-wrap {
  border: 1px solid;
  width: 600px;
  height: 600px;
  display: table;
}

.center-div {
  border: 1px solid;
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
```
