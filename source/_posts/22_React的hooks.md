---
title: React的hooks
---

纯函数组件: react15叫无状态组件

hooks组件: 逻辑和ui完全分离

### 1. 常用aip: 
memo: 会有引用传递的问题
useState: 
useMemo: 相当于计算属性
useEffect: 副作用 相当于生命周期的componentDidMount和componentWillUnmount

```javascript
  useEffect(() => { 
    console.log('副作用') // componentDidMount
    return () => {
      console.log('副作用的注销') // componentWillUnmount
    }
  }, [count]) // 可以指定, 这个参数的意思是: 只有count会引发副作用
```

useCallback: 为了解决memo引用传递的问题
context: 

###2. 封装自己的hooks

```javascript

  const useCount = (initialCount = 0) => {
    const [count, setCount] = useState(initialCount);
    return [count, () => setCount(count + 1), () => setCount(count - 1)];
  };
  export default () => {
    const [count, increment, decrement] = useCount(1);
    //首次渲染完成
    //   componentDidMount() {
    //     document.title = `You clicked ${this.state.count} times`;
    //   }
    //更新渲染完成
    //   componentDidUpdate() {
    //     document.title = `You clicked ${this.state.count} times`;
    //   }
    //组件卸载阶段 == return function useEffect每次组件变更均执行
    // componentWillUnmount(){

    // }
    useEffect(() => {
      console.log("component update");
      document.title = `标题-${count} times`;
      // fetch("")
      // increment()
      return () => {
        console.log("unbind");
      };
    }, [count]);

    return (
      <>
        <input type="button" value="增加count" onClick={increment} />
        <span>当前count: {count}</span>
        <input type="button" value="减少count" onClick={decrement} />
      </>
    );
  }
  
```

**注意:** 
1. hook的useState使用只能暴露在最外层
2. 只能在函数组件中使用hooks
3. 函数组件业务变更无需修改成class组件
4. 告别了繁杂的this和难以记忆的生命周期
5. 合并了生命周期componentDidMount, componentDidUpdate和componentWillUnmount
6. 包装自己的hooks 是基于纯命令式的api
7. useReducer集成redux
8. useEffect接受操作等到react更新了DOM之后, 它依次执行我们定义的副作用函数. 这里就是一个io 且是异步的.




