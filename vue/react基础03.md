# react基础03

Actor:shao

## 函数式编程

js语言就是函数式编程思想设计

特点

可以作为变量且可用作数据结构储存

可以作为参数传递给方法/函数

可以作为返回值从函数/方法返回

```
let fn = callback=>a=>callback(a);
```

声明式

```
let a = 10;
```

声明式的设计思想强调程序的运行结果而非运行过程

 与命令式操作相比

1、声明式代码较于命令式而言更为精简

 2、声明式代码见名知意，而命令式需要以大量注释加以辅助

 3、声明式强调结果，命令式强调过程

```
let str = "hello world nice to meet you";

    //命令式+声明式
    let arr = str.split(" ");

    let result = arr.join("-");
```

纯命令式

```
let newStr = "";//用于拼接后的新字符串的储存

    for(let i = 0 ; i < str.length ; i++){//

        if(str.charAt(i)==" "){//

            newStr+="-"//

        }else{

            newStr+=str.charAt(i)//

        }

    }
```

声明式

```
let newS = str.replace(/ /g,"-"); 
```

react采用的就是声明式设计思想

在函数式编程时，是不允许对原始数据进行修改的(不可变性)

```
let arr = [1,2,3,4];

arr.slice(0);

arr.splice(0,4);
```

纯函数

1、纯函数至少具备一个参数

2、纯函数不会产生任何副作用（不影响函数外的任何数据的改变，不影响函数外程序运行）

3、纯函数必须返回数据或函数

```
let arr = [1,2,3,4];
        function myPush(arr) { 

            arr = arr.slice(0);

            arr.push(arr.length+1);

            return arr;

        }

        console.log(myPush(arr));

        console.log(arr);
```

## 生命周期钩子函数

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

    <script src="./lib/react.development.js"></script>

    <script src="./lib/react-dom.development.js"></script>

    <script src="./lib/babel.min.js"></script>

</head>

<body>

    <div id="app">



    </div>

</body>

<script type="text/babel">

    class Comp extends React.Component {

        constructor(props){

            super();

            this.state={

                num:1,

                count:2

            }

        }

        handleClick(){

            this.setState({

                num:3

            })

        }

        render(){

            return <div>

                    <p>组件中的props：{this.props.data}</p>

                    <p>组件中的state：{this.state.num}</p>

                    <button onClick={this.handleClick.bind(this)}>num+1</button>

                </div>

        }

        componentWillReceiveProps(nextProps){

            //唯一一个可以使用setState方法的监听状态修改的生命周期钩子函数

            this.setState({



            })

            console.log("componetWillReceiveProps",nextProps);

            //监听props的改变

        }

        shouldComponentUpdate(nextProps,nextState){

            console.log("shouldComonentUpdate",nextProps,nextState,this.state);

            let bol = true;

            for(let k in this.state){

                if(this.state[k]==nextState[k]){

                    continue;

                }else{

                    bol = false;

                }

            }

            //守卫

            return !bol;

        }

        componentWillUpdate(nextProps,nextState){

            console.log("componentWillUpdate",nextProps,nextState)

            //更新前钩子

        }

        componentDidUpdate(nextProps,prevState){

            console.log("componentDidUpdate",nextProps,prevState)

            //更新后

        }

        componentWillMount(){



        }

        componentDidMount(){



        }

        componentWillUnmount(){

            

        }

    }

    let str = 1;

    // setInterval(() => {

    //     str+=1;

        ReactDOM.render(<Comp data={str}/>,document.querySelector("#app"));

    //     if(str>=3){

    //         clearInterval(1);

    //     }

    // }, 1000);

</script>

</html>
```

## 高阶组件儿的概念

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

    <script src="./lib/react.development.js"></script>

    <script src="./lib/react-dom.development.js"></script>

    <script src="./lib/babel.min.js"></script>

</head>

<body>

    <div id="app"></div>

</body>

<script type="text/babel">

    //高阶组件实际上就是一个高阶函数

    //传入一个组件作为参数，返回一个新的组件的函数就是高阶组件



    class InnerComp extends React.Component {

        render(){

            return <div>

                <p>参数组件</p>    

            </div>

        }

    }



    let compHoc = Comp =>class extends React.Component {

        render(){

            return <div>

                <h2>我是高阶组件</h2> 

                {Comp}

            </div>

        }

    }

    // let compHoc = function(Comp){

    //     return class extends React.Component {

            

    //     }

    // }

    let CompHoc = compHoc(<InnerComp/>);//高阶组件

    ReactDOM.render(<CompHoc/>,document.querySelector("#app"))

</script>

</html>
```

## 案例

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

    <script src="./lib/react.development.js"></script>

    <script src="./lib/react-dom.development.js"></script>

    <script src="./lib/babel.min.js"></script>

    <style>

        *{

            margin: 0;

            padding: 0;

        }

        .innerBox{

            width: 200px;

            height: 200px;

            background: red;

        }

    </style>

</head>

<body>

    <div id="app"></div>

</body>

<script type="text/babel">

    //高阶组件实际上就是一个高阶函数

    //传入一个组件作为参数，返回一个新的组件的函数就是高阶组件



    class InnerComp extends React.Component {

        constructor(props){

            super()

            console.log(props)

        }

        handleA(){

            console.log(2);

        }

        render(){

            return <div ref="indexBox" className="innerBox">

                 

            </div>

        }

    }



    let compHoc = Comp =>class extends React.Component {

        constructor(props){

            super(props);

            this.state = {

                ...props

            }

        }

        handleMove(ev){

            console.log(ev.clientX,ev.clientY);

        }

        render(){ 

            return <div>

                <Comp ref="comp" />

            </div>

        }

        componentDidMount(){

            //refs只能在挂载后读取

            console.log(this);

            this.refs.comp.refs.indexBox.onmousemove = this.handleMove

        }

    }

    

    let CompHoc = compHoc(InnerComp);//高阶组件

    ReactDOM.render(<CompHoc/>,document.querySelector("#app"))

</script>

</html>
```

## axios基础用法

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

    <script src="./lib/react.development.js"></script>

    <script src="./lib/react-dom.development.js"></script>

    <script src="./lib/babel.min.js"></script>

    <script src="./lib/axios.js"></script>

</head>

<body>

    <!-- axios 不属于任何框架，属于独立插件，用于前后端数据交互 -->

    <div id="app"></div>

</body>

<script type="text/babel">

//get请求

    axios({

        method:"get",

        url:"http://localhost:8080/cs",

        params:{

            user:"lee",

            pass:"123"

        }

    })

    .then(res=>{

        console.log(res);

    })

    .catch(err=>{

        console.log(err);

    })



//post

    //在跨域访问时   post提交的数据需要通过URLSearchParams对象进行数据格式转换

    let params = new URLSearchParams();//

    // params.append(key,val)

    params.append("user","leepost");

    params.append("pass","123");

    axios({

        method:"post",

        url:"http://localhost:8080/cs",

        data:params,

        headers:{

            "content-type":"application/x-www-form-urlencoded"

        }

    })

    .then(res=>{

        console.log(res);

    })



</script>

</html>
```

