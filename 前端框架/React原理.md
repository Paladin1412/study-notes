### React类
- jsx
什么是jsx？
jsx是对js语法的扩展，使我们可以用类似xml方式描述视图，执行速度快，类型安全，提高开发效率
原理: babel-loader会预编译jsx为React.createElement(type, props, ...children)

```
function Comp(props) {
    return <h2>h1, {props.name}</h22>
}
const jsx = (
    <div id="demo">
        <span>hi</span>
        <Comp name="yindong"/>
    </div>
);
console.log(jsx);
ReactDOM.render(jsx, document.querySelector('#root'));
```