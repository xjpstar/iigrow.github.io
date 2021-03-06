---
layout: page
title: "JSX基础语法"
date: 2016-03-11 22:14
categories: React/JSX
---

React的本质上是一个状态机：对于特定的输入，总是会返回一致的输出

React 中组件是用于分离关注点的，React鼓励为每一个关注点创造一个独立的组件，并把所有的逻辑和标签封装在其中。

1. JSX将{...}中的内容渲染为动态之，在大括号中放入的任何东西都会被进行求值
2. React通过将数组中的每个元素渲染为一个节点的方式对数组进行自动求值
3. React将开始标签与结束标签之间的所有子节点保存在一个名为this.props.children的特殊组件属性中


JSX是javascript语法的扩展。
JSX可以用简洁和熟悉的语法用属性定义树结构

* 定义组件类:

通过定义组件类可有效的复用代码

    React.createClass({
       render:function(){
            return <div></div>;
        }
    });
    
* 定义组件元素:

可通过以下方式创建组件类所对应的组件实例


    React.createElement([tag],[props],[text]|[subnode1[,subnode2]]);
    
    var child1 = React.createElement('li', null, '1');
    var child2 = React.createElement('li', null, '2');
    var root = React.createElement('ul', { id:"testid"}, child1, child2);
    
* 定义组件工厂:
 
通过组件工厂可有效的缩减代码量
 
 
    var myDiv=React.createClass();
    var myDivFactory = React.createFactory(myDiv);
    var div1=myDivFactory([props],[text]|[subnode1]);
React.DOM.* 命名空间下提供了一系列的工厂，这些预定义的工厂是预置了第一个参数的React.createElement的简写


    React.DOM.div(); // React.createElement('div');
    React.DOM.hr(); // React.createElement('hr');
    
* 渲染一个组件:


当标签以小写字母开头时，渲染HTML标签:
  
  
    var customDiv = <div></div>;
    ReactDOM.render(customDiv,document.getElementById('example'));
当标签是以大写字母开头的本地变量时渲染React组件:
  
  
    var CustomComponent=React.createClass(/**/); 
    ReactDOM.render(<CustomComponent />,document.getElementById('example'));
    
* 设置属性:
    
    
将对象的属性设置为组件的属性值，如下:

    var props = { foo: 'default' };
    var component = <Component {...props} foo={'override'} />; 
    //此处属性值顺序是重要，后面的属性值将会覆盖前面的同名属性
    //JSX会将两个花括号之间的内容渲染为动态值，花括号指明了一个JavaScript上下文环境
    console.log(component.props.foo); // 输出 override
非DOM属性

1. key:是可选的唯一标示符，对于DOM刷新时的性能优化比较重要。
2. ref:允许父组件在render方法之外保持一个对子组件的引用，可以在组件中的任何地方使用this.refs.XXX获取对应的引用。
3. dangerouslySetInnerHTML:将HTML内容设置为字符串，通过把字符串设置为_html为主键的对象里，才会起作用。

注:

    render:function(){
        var htmlString = { __html:"<span></span>" };
        return <div dangerouslySetInnerHTML={htmlString} ></div>; // 注意使用该属性时，标签的InnerHtml必须为空
    }

特殊属性

1. 添加for属性时用htmlFor
2. 自定义class用className
3. React将开始标签与结束标签之间的所有节点保存在一个名为this.props.children的特殊属性中
    
注:

    <CustomComponent><span>abc</span></CustomComponent>
    // this.props.children为<span>abc</span>

* 挂接事件:

事件名统一规范用驼峰式表示，例如click为onClick，React自动绑定了组件所有方法的作用域。

    render:function(){
        return <div onClick={this.handleClick}>...</div>
    }

> 默认情况下事件都是在冒泡阶段被触发，如果要在捕获阶段触发，需要对事件名添加Capture后缀，比如：onClickCapture

* 设置样式

React把所有的内联样式都规范化为了驼峰形式，例如:

    var styles={ borderColor:red };
    React.renderComponent(<div style={styles}>...</div>,);
    
### 注:JSX在编译后都会转化为原生的javascript代码:

    var Father, Child;
    var app = <Father id="testid"><Child>world</Child></Father>;
    var app = React.createElement(
                    Father,
                    {id:"testid"},
                    React.createElement(Child, null, "world")
                );