# react基础02

actor:shao

## react输入框取值

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

</head>

<body>

    <div id="app"></div>

</body>

<script src="./lib/react.development.js"></script>

<script src="./lib/react-dom.development.js"></script>

<script src="./lib/babel.min.js"></script>

<script type="text/babel">

    class Ipt extends React.Component{

        constructor(props){

            super(props)

            this.state={

                a:1,

                num:2,

                count:3,

                formData:{}

            }

        }

        handleInput(ev){

            let el=ev.target;

            this.setState({

                formData:Object.assign(this.state.formData,{[el.name]:el.value})

            })

        }

        render(){

            return (<div>

                <input type="text" name="age" onInput={this.handleInput.bind(this)}/>

                <input type="text" name="address" onInput={this.handleInput.bind(this)} />

                <input type="text" name="tel" onInput={this.handleInput.bind(this)}/>

                <input type="text" name="sex" onInput={this.handleInput.bind(this)}/>

                <input type="text" name="work" onInput={this.handleInput.bind(this)}/>

                <input type="text" name="name" onInput={this.handleInput.bind(this)}/>        

                <button onClick={()=>{

                    console.log(this.state)

                }}>提交</button>

                </div>)

        }

    }

    ReactDOM.render(<Ipt/>,app)

</script>

</html>
```

## react单选框

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

</head>

<body>

    <div id="app"></div>

</body>

<script src="./lib/react.development.js"></script>

<script src="./lib/react-dom.development.js"></script>

<script src="./lib/babel.min.js"></script>

<script type="text/babel">

    class RadioIpt extends React.Component{

        constructor(props){

            super(props)

            this.state={

                sex:"boy"

            }

        }

        handleChange(ev){

            console.log(ev.target)

            this.setState({

                sex:ev.target.value

            },()=>{

                console.log(this.state.sex)

            })

        }

        render(){

            return (<div>

                    <label htmlFor="girl">女</label> <input type="radio" defaultChecked={this.state.sex=="girl"} onChange={this.handleChange.bind(this)} value="girl" name="sex"/>

                    <label htmlFor="boy">男</label> <input type="radio" defaultChecked={this.state.sex=="boy"} onChange={this.handleChange.bind(this)} value="boy" name="sex"/>

                </div>)

        }

    }

    ReactDOM.render(<RadioIpt/>,app)

</script>

</html>
```

## 多选框以及案例

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

</head>

<body>

    <div id="app"></div>

</body>

<script src="./lib/react.development.js"></script>

<script src="./lib/react-dom.development.js"></script>

<script src="./lib/babel.min.js"></script>

<script type="text/babel">

    class CheckBoxIpt extends React.Component{

        constructor(){

            super()

            this.state={

                chooseArr:["4","5","6"]

            }

        }

        handleChange(ev){

            let el=ev.target;

            let val=el.value;

            let index=this.state.chooseArr.indexOf(val)

            index==-1?this.state.chooseArr.push(val):this.state.chooseArr.splice(index,1);

            this.setState({

                chooseArr:this.state.chooseArr

            })

        }

        handleAllChoose(e){

            this.setState({

                chooseArr:["1","2","3","4","5","6"]

            })

        }

        render(){

            return (<div>

                <input type="checkbox" value="1" checked={this.state.chooseArr.includes("1")} onChange={this.handleChange.bind(this)}/>1<br/>  

                <input type="checkbox" value="2" checked={this.state.chooseArr.includes("2")} onChange={this.handleChange.bind(this)} />2 <br/>

                <input type="checkbox" value="3" checked={this.state.chooseArr.includes("3")} onChange={this.handleChange.bind(this)} />3 <br/>

                <input type="checkbox" value="4" checked={this.state.chooseArr.includes("4")} onChange={this.handleChange.bind(this)} />4 <br/>

                <input type="checkbox" value="5" checked={this.state.chooseArr.includes("5")} onChange={this.handleChange.bind(this)} />5 <br/>

                <input type="checkbox" value="6" checked={this.state.chooseArr.includes("6")} onChange={this.handleChange.bind(this)} />6 <br/>

                <button onClick={this.handleAllChoose.bind(this)}>全选</button>

                <button onClick={()=>{

                    let all=["1","2","3","4","5","6"]

                    this.setState({

                        chooseArr:all.filter((item,index)=>{

                            return !this.state.chooseArr.includes(item)

                        })

                    })

                }}>反选</button>

                <button onClick={()=>{

                    this.setState({

                        chooseArr:[]

                    })

                }}>全不选</button>

                </div>)

        }

    }

    {

        let arr=["1","2","3"]

        console.log(arr.indexOf("2"))

        console.log(arr.splice(arr.indexOf("2"),1))

    }

    ReactDOM.render(<CheckBoxIpt/>,app)

</script>

</html>
```

## react留言板

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

</head>

<body>

    <div id="app"></div>

</body>

<script src="./lib/react.development.js"></script>

<script src="./lib/react-dom.development.js"></script>

<script src="./lib/babel.min.js"></script>

<script type="text/babel">

    class Lyb extends React.Component {

        constructor() {

            super()

            this.state = {

                isShow: false,

                title: "",

                content: "",

                list: [],

                chooseArr: []

            }

        }

        handleInput(ev) {

            this.setState({

[ev.target.name]: ev.target.value

            })

        }

        handleChange(ev) {

            let el = ev.target;

            let val = el.value;

            let index = this.state.chooseArr.indexOf(val);

            index == -1 ? this.state.chooseArr.push(val) : this.state.chooseArr.splice(index, 1);

            this.setState({

                chooseArr: this.state.chooseArr

            }, () => {

                console.log(this.state.chooseArr)

            })

        }

        render() {

            return (<div>

                <input type="text" name="title" value={this.state.title} onInput={this.handleInput.bind(this)} /><br />

                <textarea type="text" name="content" value={this.state.content} onInput={this.handleInput.bind(this)} ></textarea>

                <button onClick={

                    () => {

                        this.state.list.push({

                            title: this.state.title,

                            content: this.state.content

                        })

                        this.setState({

                            list: this.state.list

                        }, () => {

                            this.setState({

                                title: "",

                                content: ''

                            })

                        })

                        console.log(this.state.list)

                    }

                }>提交</button>

                {/*页面渲染*/}

                <ul>

                    {this.state.list.map((item, index) => {

                        return <li key={index}>

                            {this.state.isShow ? <input type="checkbox" value={index} onChange={this.handleChange.bind(this)} checked={this.state.chooseArr.includes(index.toString())} /> : null}

                            <h2>{item.title}</h2>

                            <p>{item.content}</p>

                            <button onClick={() => {

                                this.state.list.splice(index, 1);

                                this.setState({

                                    list: this.state.list

                                })

                            }}>删除</button>

                        </li>

                    })}

                </ul>

                {/*批量删除按钮*/}

                <button onClick={() => {

                    this.setState({

                        isShow: true

                    })

                }}>批量删除按钮</button>

                {/*删除选中项  取消*/}

                {this.state.isShow ? <div>

                    <button onClick={() => {

                        this.setState({

                            list: this.state.list.filter((item, index) => {

                                return !this.state.chooseArr.includes(index.toString())

                            }),

                            isShow: false,

                            chooseArr: []

                        })

                    }}>删除选中项</button>

                    <button onClick={() => {

                        this.setState({

                            isShow: false,

                            chooseArr: []

                        })

                    }}>取消</button>

                </div> : null}

                {/*全选，反选，全不选*/}

                {this.state.isShow ? <div>

                    <button onClick={() => {

                        for (let i = 0; i < this.state.list.length; i++) {

                            this.state.chooseArr[i] = i.toString();

                        }

                        this.setState({

                            chooseArr: this.state.chooseArr

                        })

                    }}>全选</button>

                    <button onClick={() => {

                        let arr = [];

                        for (let i = 0; i < this.state.list.length; i++) {

                            arr[i] = i.toString();

                        }

                        this.setState({

                            chooseArr: arr.filter((item, index) => {

                                return !this.state.chooseArr.includes(item);

                            })

                        })

                    }} >

                        反选

                </button>

                    <button onClick={() => {

                        this.setState({

                            chooseArr: []

                        })

                    }} >

                        全不选

                </button>

                </div> : null}

            </div>)

        }

    }

    ReactDOM.render(<Lyb />, app)

</script>

</html>
```

## react生命周期

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Document</title>

</head>

<body>

    <div id="app"></div>

</body>

<script src="./lib/react.development.js"></script>

<script src="./lib/react-dom.development.js"></script>

<script src="./lib/babel.min.js"></script>

<script type="text/babel">

    class Comp extends React.Component{

        constructor(){

            super()

            this.state={

                isShow:true

            }

            console.log("contructor","创建后")

        }

        //挂载前

        componentWillMount(){

            console.log("will mount","准备挂载",document.querySelector(".div1"))

        }

        render(){

            //只要当前组件的state状态被修改，且影响dom渲染则render函数就会被触发

            console.log("render","开始挂载")

            return <div className="div1">comp组件

                <button onClick={()=>{

                    this.setState({

                        isShow:!this.state.isShow

                    })

                }}>isShow</button>

                {this.state.isShow?<ChildrenComp/>:null}

            </div>

        }

        //挂载后的生命周期钩子函数

        componentDidMount(){

            console.log("did mount","挂载后",document.querySelector(".div1"))

        }

        //卸载前的生命周期钩子函数

        componentWillUnmount(){

            console.log("will unmount","卸载前")

        }

    }

    class ChildrenComp extends React.Component {

        render(){

            return <p>子组件</p>

        }

        componentWillUnmount(){

            //只有当前组件不被渲染到页面时触发

            console.log("will unmount","卸载前")

        }

    }

    ReactDOM.render(<Comp/>,app)

</script>

</html>
```

## react时钟案例

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

    let app = document.querySelector("#app")

    class Timer extends React.Component {

        constructor(){

            super();

            this.state = {

                time:new Date().getTime()

            }

        }

        timePicker(){

            let time = new Date(this.state.time);

            return time.getHours()+":"+time.getMinutes()+":"+time.getSeconds();

        }

        componentDidMount(){

            setInterval(()=>{

                this.setState({

                    time:new Date().getTime()

                })

            },1000)

            

        }

        render(){

            return <p>现在时间：{this.timePicker.call(this)}</p>

        }

    }

    ReactDOM.render(<Timer/>,app)

    

</script>

</html>
```

