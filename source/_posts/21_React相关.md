---
title: React相关
---

### 1. React为什么不能返回多个组件?
只能返回三种形式: 
1). 字符串
2). dom
3). React.createClass

因为React不支持, 是jsx的语法糖
实际上返回的: 
```javascript
return React.CreateClass(h4, {className; 'text-warning})
```

### 2. React的抽象代码能力是怎么进化的?

mixin
缺点: 难以维护, 命名冲突

hoc(高阶组件)
优势: 不会有命名冲突
缺点: 层级深, props不知道从哪里传过来的, 封装一些ui在里面, 复用很难

renderProps
和高阶组件的理念很像

Hook
优势: ui和逻辑完全的分离

### 3. PureComponent

PureComponent是优化react应用程序最重要的方法，可以减少不必要的render次数，提高性能。
原理:
组件发生更新时，组件的props和state没有改变，render方法就不会触发，省去虚拟dom生成和对比的过程，其实就是react自动帮忙做了一个浅比较

```javascript
  if(this._compositeType === CompositeTypes.PureClass){
    shouldUpdate = !shallowEqual(prevProps,nextProps)||!shallowEqual(inst.state,nextState)
  }
```

而 shallowEqual 又做了什么呢？会比较

1. Object.keys(state | props) 的长度是否一致，
2. 每一个 key 是否两者都有，
3. 是否是一个引用。
也就是只比较了第一层的值，确实很浅，所以深层的嵌套数据是对比不出来的。

**易变数据不能使用一个引用**
数据类型如果是引用类型，需要注意
```javascript
  import React,{PureComponent,Component} from 'react'
  class App extends Component/PureComponent {
    state = {
      items: [1, 2, 3]
    }
    handleClick = () => {
      const { items } = this.state;
      items.pop();
      this.setState({ items });
    }
    render() {
      return (<div>
        <ul>
          {this.state.items.map(i => <li key={i}>{i}</li>)}
        </ul>
        <button onClick={this.handleClick}>delete</button>
      </div>)
    }
  }

  export default App;
```

**与 shouldComponentUpdate 共存**
如果 PureComponent 里有 shouldComponentUpdate 函数的话，直接使用 shouldComponentUpdate 的结果作为是否更新的依据，没有 shouldComponentUpdate 函数的话，才会去判断是不是 PureComponent ，是的话再去做 shallowEqual 浅比较。

```javascript
  // 这个变量用来控制组件是否需要更新
  var shouldUpdate = true;
  // inst 是组件实例
  if (inst.shouldComponentUpdate) {
    shouldUpdate = inst.shouldComponentUpdate(nextProps, nextState, nextContext);
  } else {
    if (this._compositeType === CompositeType.PureClass) {
      shouldUpdate = !shallowEqual(prevProps, nextProps) ||
        !shallowEqual(inst.state, nextState);
    }
  }
```


### 4. 高阶组件

高阶组件就是一个没有副作用的纯函数, 这个函数接收compoent为参数, 返回一个处理后的component

```javascript
  const wrapWithUsername = (WrappedComponet) => {
    class NewComponent extends Component {
      constructor() {
        super()
        this.state = {
          username: ''
        }
      }

      componentWillMount() {
        const username = localstorage.getItem('username')
        this.setState{
          username: username
        }
      }

      render() {
        return <WrappedComponet username={this.state.username} />
      }
    }
    return NewComponent
  }

  class Welcome extends Component {
    render () {
      return <div>welcome, {this.props.username}</div>
    }
  }
  class Goodbey extends Component {
    render () {
      return <div>goodbey, {this.props.username}</div>
    }
  }
  // 升级高阶组件
  Welcome = wrapWithUsername(Welcome)
  Goodbey = wrapWithUsername(Goodbey)

  class Greeting extends Component {
    render() {
      return <Welcome /> <Goodbey />
    }
  }

```
### 5. renderProps

一个用于告知组件需要渲染什么内容的函数
具有 render prop 的组件接受一个函数，该函数返回一个 React 元素并调用它而不是实现自己的渲染逻辑。

```javascript
  class RenderProp extends Component {
    render() {
      const username = localStorage.getItem('username')
      return this.props.children(username)
    }
  }

  class Welcome extends Component {
    render() {
      return <div>welcome, {this.props.username}</div>
    }
  }
  class Goodbye extends Component {
    render() {
      return <div>goodbye, {this.props.username}</div>
    }
  }

  class Greeting extends Component {
    render() {
      return (
        <>
          <RenderProp>
            {
              (username) => {
                return (
                  <>
                    <Welcome username={username} />
                    <Goodbye username={username} />
                  </>
                )
              }
            }
          </RenderProp> 
        </>
      )
    }
  }
```

### 6. 组件插槽

Portals 提供了一个顶级的方法，使得我们有能力把一个子组件渲染到父组件 DOM 层级以外的 DOM 节点上。

```javascript
  // 组件插槽
  const portalElm = document.createElement('div')
  portalElm.className = 'txtcenter'
  document.body.appendChild(portalElm)

  class App extends React.Component {
    state = {
      show: true,
    }

    handleClick = () => {
      this.setState({
        show: !this.state.show,
      })
    }

    render() {
      return (
        <div>
          <button className="btn btn-primary" onClick={this.handleClick}>动态展现Portal组件</button>
          {this.state.show ? (
            <div>{ReactDOM.createPortal(<span>Portal组件</span>, portalElm)}</div>
          ) : null}
        </div>
      )
    }
  }
```

### 7. Suspense?

Suspense: 使得组件可以“等待”某些操作结束后，再进行渲染。目前，Suspense 仅支持的使用场景是：通过 React.lazy 动态加载组件。它将在未来支持其它使用场景，如数据获取等。

React.lazy() 允许你定义一个动态加载的组件。这有助于缩减 bundle 的体积，并延迟加载在初次渲染时未用到的组件。
React.Suspense 可以指定加载指示器（loading indicator），以防其组件树中的某些子组件尚未具备渲染条件。目前，懒加载组件是 <React.Suspense> 支持的唯一用例：

### 8. (hook的大原理)同步实现异步函数 ? hook的实现原理 github有一个人出了一个文章

### 9. memo

React.memo() 为高阶组件。它与 React.PureComponent 非常相似，但它适用于函数组件，但不适用于 class 组件。
此方法仅作为性能优化的方式而存在

### 10. context

Context 主要是解决props向多层嵌套的子组件传递的问题，原理是定义了一个全局对象

### 11. Ref有几种使用方式
三种写法: 

```javascript
  1). ref='sfeafdf' this.refs['dfjslfkdsl']
  2). <TargetComponent ref={(ref) => this.targetRef = ref}/>
  3). this.ref = React.createRef()
```

### 12. Error错误组件

之前如果react没有捕获到报错的话, react会销毁到dom, 页面会白屏.
在最外层的父组件加一层生命周期
```javascript
  // 补货错误和错误上报程序库一起适应
  componentDidCatch(err, info) {
    this.setState({hasError: true})
  }
```

### 13. react的生命周期(面试必问)

加载 更新 卸载

卸载是什么时候执行的?

react15生命周期: 
![image](https://note.youdao.com/yws/api/personal/file/WEB5ee888cf2d0b58bdecaf27e54f921322?method=download&shareKey=3170f756804915b1570de83ec888b7f5)

react15生命后期流程: 
![image](https://note.youdao.com/yws/api/personal/file/WEB1dd3844bfb85e5211aa9a46ca77e0e07?method=download&shareKey=f6b19683eaf7206aa1a49176e0219fe5)

react16生命周期: 
![image](https://note.youdao.com/yws/api/personal/file/WEB8611b3cd0e0727c86ab0beb695f98eb9?method=download&shareKey=43b6bfd91cc3965b514d3e13f07ca663)

react16生命周期-1: 
![image](https://note.youdao.com/yws/api/personal/file/WEB4156657eac36b94e1be5a1649a94d172?method=download&shareKey=81c874f2b5937fcc477a677ca1c21f70)

state可用周期: 
![image](https://note.youdao.com/yws/api/personal/file/WEBd51157cb272e4f065d0a251de60bb6be?method=download&shareKey=bca1453a326ac526121642e02a0f3579)

### 14. React的fiber

一个切片, 

### 15. setState有几种传参形式
```javascript
  // 三种形式 普通 callback fiber
  setState({a: 1}, () => { this.state })

  setState((state) => {
    return {...state}
  })
```

### 16. react的合成事件
### 17. 简单的diff
### 18. 简单的diff
### 19. 生命周期


## redux
### 1. 为什么reducer里面每次都要返回一个状态出来?

因为reducer每次都会执行







