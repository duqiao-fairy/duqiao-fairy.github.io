---
title: css上下左右居中
---

## 方法一：transform
> left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);

完整代码：
```javascript
  .wrap {
    width: 500px;
    height: 500px;
    border: 1px solid #000;
  }
  .content {
    position: relative;
    left: 50%;
    top: 50%;
    width: 200px;
    height: 200px;
    transform: translate(-50%,-50%);
    border: 1px solid blue;
  }
```

## 方法二：绝对定位
> position: absolute;
  margin: auto;
  top:0;
  right:0;
  bottom:0;
  left:0;

完整代码：
```javascript
  .wrap {
    position: relative;
    width: 500px;
    height: 500px;
    border: 1px solid #000;
  }
  .content {
    position: absolute;
    margin: auto;
    width: 200px;
    height: 200px;
    top:0;
    right:0;
    bottom:0;
    left:0;
    border: 1px solid blue;
  }
```

## 方法三：Flex布局
> display: flex;
  align-items: center;
  justify-content: center;

完整代码：
```javascript
  .wrap {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 500px;
    height: 500px;
    border: 1px solid #000;
  }
  .content {
    width: 200px;
    height: 200px;
    border: 1px solid blue;
  }
```

