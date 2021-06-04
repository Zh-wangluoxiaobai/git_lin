# react基础01

actor:shao

## jsx基础

jsx是什么？

jsx：

​            JavaScript XML

​            jsx属于JavaScript的一种语法糖

​            （在某语言的语法基础上的二次封装，使用时比原语法更加简单方便，

​            但是无法直接被识别读取，所以需要一定的垫片处理）；

​            在script脚本中正常书写标签结构，这就是jsx

​            jsx中仅可出现一个根元素

​            推荐使用jsx来完成页面渲染，渲染速度优于createElement方式

<!-- 若在脚本中使用jsx  需将script的type 属性修改为“text/babel” -->

 外部导入使，type也需要进行设置  服务器环境下

```
<script type="text/babel" src="./lib/my_jsx.js" ></script>
```

```
ReactDOM.render(<h1>jsx就是这个样的</h1>,document.querySelector("#app"));
       const dom = (<ul>
           <li>
               <h2>一级菜单</h2>
               <ul>
                   <li>
                       <h3>二级菜单</h3>
                       <ul>
                           <li><h4>三级菜单</h4></li>
                           <li><h4>三级菜单</h4></li>
                           <li><h4>三级菜单</h4></li>
                       </ul>
                   </li>
                   <li>
                       <h3>二级菜单</h3>
                       <ul>
                           <li>
                               <h4>三级菜单</h4>
                               <ul>
                                   <li><h5>四级菜单</h5></li>
                                   <li><h5>四级菜单</h5></li>
                               </ul>
                           </li>
                       </ul>
                   </li>
               </ul>
           </li>

       </ul>)

    ReactDOM.render(dom2,document.querySelector("#app"))
```

jsx的表达式  读取js变量或js语法 {}

在表达式中不允许使用if elseif 语法

要想实现逻辑判断，可以使用三目运算符

标签中添加类名仍使用className添加

```
const {render} = ReactDOM;

    const str = "今天天不错";

    let style1 = {

        width:"200px",

        height:"200px",

        background:"green"

    }

    let arr = [{

        title:"农夫山泉",

        price:"¥2"

    },{

        title:"怡宝",

        price:"¥2"

    }]

    // map方法实现遍历

    // const dom = <h1 id="main" className="xxx"></h1>

    // 将数据数组映射为jsx数组

    const ul = (<ul>

            {arr.map((item,index)=>{

                return <li key={index}>

                    <h2 style={style1}>{item.title}</h2>

                    <h3>{item.price}</h3>

                </li>

            })}

        </ul>)

    render(ul,document.querySelector("#app"))
```

## 无状态组件儿

####  无状态组件特点

​     \*  1、无状态组件不会被实例化，渲染性能更高一些

​     \*  2、无状态组件一般只作用于某一个dom结构的封装，纯粹的渲染功能

​     \*  3、无状态组件中没有state状态（当前组件的私有状态）

​     \*  4、无状态组件中没有this指针，只具备一个props参数（只读属性的参数），不能被修改

​     \*  5、react推荐使用无状态组件来完成页面渲染（能使用无状态组件的前提下，尽量使用无状态组件）

#### 无状态组件语法

函数时声明

```
function MyComp() {
     return <h1>我是无状态组件</h1>
 }
```

箭头式声明

```
const MyComp = ()=>(<h1>我是无状态组件</h1>);
    ReactDOM.render(<div>
    {/** 函数式调用*/}
    {MyComp()}
    {/** 标签式调用 推荐使用*/}
    <MyComp/>
</div>,document.querySelector("#app"))
```

无状态组件props

```
const MyComp = (props)=>{
        //props只是一个只读对象，严禁修改
        // props.diy = "zzzz";//报错
        return <h1>{props.diy}</h1>
    }
    ReactDOM.render(<div>
        <MyComp diy="xxx" />
    </div>,document.querySelector("#app"))
```

无状态组件儿案例

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <script src="./lib/react.development.js"></script>

    <script src="./lib/react-dom.development.js"></script>

    <script src="./lib/babel.min.js"></script>

    <title>Document</title>

</head>

<body>

    <div id="app">
    </div>

</body>

<script type="text/babel">

    let arr = [{

        url:"./01.jpg",

        title:"树苗",

        option:"杨树苗",

        price:"1000"

    },{

        url:"./02.jpg",

        title:"躺椅",

        option:"长躺椅",

        price:"200"

    },{

        url:"./03.png",

        title:"袁老师英语课堂",

        option:"教你英语",

        price:"50"

    },{

        url:"./04.jpg",

        title:"张学广照片",

        option:"写真照",

        price:"-100"

    }]



    const GoodsItem = (props)=>{

        /**

         \*  props.url  图片路径

         \*  props.title  商品名称

         \*  props.option 商品详情

         \*  props.price 商品价格 

         */

        return (<div>

            <img src={props.url} />

            <h2>{props.title}</h2>

            <h3>{props.option}</h3>

            <h3>¥{props.price}</h3>

        </div>)

    }

    ReactDOM.render(<div>

        {

            arr.map((item,index)=>{

                return <GoodsItem key={index} url={item.url} title={item.title} option={item.option} price={item.price} />

            })

        }

    </div>,document.querySelector("#app"))

</script>

</html>
```

## 复合组件

复合组件就是无状态组件的嵌套调用

```
 let CompOne = (props)=><h2>我是第一个无状态组件{props.data}</h2>

    let CompTwo = (props)=>(<div>

        <p>我是第二个无状态组件</p>

        <CompOne data={props.data} />

    </div>)

    ReactDOM.render(<CompTwo data="哈哈哈" />,document.querySelector("#app"))
```

## 有状态组件儿

state:用来保存组件的私有状态（组件中可变的参数）

```
 class Comp extends React.Component {

        constructor(props){

            super();

            //手动注册state状态

            this.state = {

                num:1

            }

            // this=>Comp实例

        }

        render(){

            console.log(this);

            // 访问props时  通过this指针访问  this.props.xxx

            // 在有状态组件中props仍然是只读属性

            // this=>Comp实例

            // 返回jsxdom模板

            return <h1>我是有状态的 num为{this.state.num}</h1>

        }

    }

    ReactDOM.render(<Comp title="xx"/>,document.querySelector("#app"));
```

## 设置有状态组件儿的状态

```
class Comp extends React.Component {

        constructor(props){

            super(props);

            //可以将props数据注册到state里，方可修改

            this.state = {

                num:1,

                title:this.props.title

            }

        }

        render(){

            return <div>

                <p>{this.state.num}</p>

                <button onClick={()=>{

                    // console.log("run");

                    // console.log(this);

                    this.setState({

                        num:this.state.num+1

                    })

                    console.log(this);

                }}>state.num++按钮</button>

            </div>

        }

    }

    ReactDOM.render(<Comp />,document.querySelector("#app"))
```

## 事件绑定

//直接绑定

```
    class Comp1 extends React.Component {

        constructor(props){

            super()

            this.state = {

                num :1

            }

        }

        render(){

            return <div>

                <p>{this.state.num}</p>

                <button onClick={(ev)=>{

                    //ev.target获取目标元素进行操作

                    // console.log(ev.target);

                    this.setState({

                        num:this.state.num+1

                    })

                }} >+1</button>

            </div>

        }

    }
```

手动绑定 推荐使用

```
 class Comp2 extends React.Component {

        constructor(props){

            super()

            this.state = {

                num :1

            }

        }

        handleClick(a,b,c,ev){

            //最后一个形参为事件对象

            console.log(a,b,c,ev)

            console.log(111)

            console.log(this);

            this.setState({

                num:this.state.num+1

            })

        }

        render(){

            return <div>

                <p>{this.state.num}</p>

                <button onClick={this.handleClick.bind(this,1,2,3)}>+1</button>

            </div>

        }

    }
```

手动绑定

```
class Comp3 extends React.Component {

        constructor(props){

            super()

            this.state = {

                num :1

            }

            //将原型上的函数添加到组件实例自身

            this.handleClick = this.handleClick.bind(this,1,2,3);

        }

        handleClick(a,b,c,ev){

            console.log(a,b,c,ev)

            console.log(this);

            this.setState({

                num:this.state.num+1

            })

        }

        render(){

            return <div>

                <p>{this.state.num}</p>

                <button onClick={this.handleClick}>+1</button>

            </div>

        }

    }
```

箭头函数

```
class Comp4 extends React.Component {

        constructor(props){

            super()

            this.state = {

                num :1

            }

        }

        handleClick=(ev)=>{

            console.log(111)

            console.log(this);

            this.setState({

                num:this.state.num+1

            })

        }

        render(){

            return <div>

                <p>{this.state.num}</p>

                <button onClick={this.handleClick}>+1</button>

            </div>

        }

    }
```

