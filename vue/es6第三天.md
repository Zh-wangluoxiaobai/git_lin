# es6第三天

## actor:shao

## promise

### promise含义

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

`Promise`对象有以下两个特点。

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### 基本用法

#### es5中

```
//回调方法

       function runAsyan(fn){

           setTimeout(()=>{

               console.log("今天你们跑圈了");

               fn("跑完圈感想如何?");

           },2000)
       }

    //函数调用

       runAsyan(function(data){

           console.log(data);

       })
```

#### es6中

```
//在这个时候es6引入了promise，很好的解决了回调函数的理解   

    //异步编程  ajax

    //Promise  构造函数  也是一个对象

    //console.dir(Promise);



    function runAsyan(){

        // resolve   pending=>resolve

        // reject    pending=>reject

        var p=new Promise(function(resolve,reject){

            setTimeout(()=>{

                //console.log("今天你们跑圈了");

                resolve("跑完圈感想如何？");

            },2000)

        })

        //返回promise函数

        return p;

    }

    //函数调用  then 然后

       runAsyan().then(function(data){

           console.log(data)

       })
```

#### 连续掉用

```
function runAsyan2(){

        // resolve   pending=>resolve

        // reject    pending=>reject

        return new Promise(function(resolve,reject){

            setTimeout(()=>{

                //console.log("今天你们跑圈了2");

                resolve("跑完圈感想如何2？");

            },2000)

        })

        //返回promise函数

    }



       runAsyan()// .then((data)=>{

           console.log(data);           return runAsyan2();

       }).then((data)=>{

           console.log(data);

       })
```

#### 案例1

生成一个随机数 0-10 >=5代表成功  <5的就失败

```
function ifRand(){

           let p=new Promise((resolve,reject)=>{

               //生成一个0-10的随机数

               let num=Math.floor(Math.random()*11);

               console.log(num);

               if(num>=5){

                   resolve("成功");

               }else{

                   reject("失败的原因是数字小于5");

               }

           })

           return p;

       }



       ifRand().then(function(data){

           console.log(data)

       },function(reason){

           console.log(reason);

       })
```

#### 案例2

```
读取文件

//jquery读取文件

		//aaa bbb ccc

		//		$.get("./a.html", function(data) {

		//			console.log(data);

		//			$.get("./b.html", function(data) {

		//				console.log(data);

		//				$.get("./c.html", function(data) {

		//					console.log(data);

		//				})

		//			})

		//		})



//				function con(data){

//					console.log(data)

//				}

		//promise 解决回调地狱

				new Promise((resolve, reject) => {

					$.get("./a.html", function(data) {

						console.log(data)

						resolve();

					})

				}).then(function() {

					//return runAsyan2()

					return new Promise(function(resolve) {

						$.get('b.html', function(datab) {

							console.log(datab);

							resolve();

						});

					});

				}).then(function() {

					$.get("./c.html", function(datac) {

						console.log(datac)

					})

				})
```

#### 案例3

```
promise封装ajax

function getjson(url){

        return new Promise((resolve,reject)=>{

            let xhr=new XMLHttpRequest();

            xhr.open("GET",url,true);

            xhr.send();

            xhr.onreadystatechange=function(){

                if(this.readyState==4){

                	

                    if(this.status==200){

                        resolve(this.responseText)

                    }else{

                        //status

                        let status=this.status;

                        reject(status);

                    }

                }

            }

            

        })  

    }
```

#### promise.all()

```
//promise 的all方法 接收一个数组，里面的值最终都返回到results中。也是数组

    Promise.all([runAsyan(),runAsyan2()]).then(function(results){

        console.log(results);

    })
```

#### promise.race()

```
 Promise.all([runAsyan(),runAsyan2()]).then(function(results){
        console.log(results);
    })
```



#### promise.catch()

```javascript
const promise = new Promise(function(resolve, reject) {
  throw new Error('test');
});
promise.catch(function(error) {
  console.log(error);
});
// Error: test
上面代码中，promise抛出一个错误，就被catch方法指定的回调函数捕获。
```

#### promise.finally

## iterator

```
自己封装

function iterator(arr){

           let index=0;

           return {

               next:function(){

                   return index<arr.length?{value:arr[index++],done:false}:{value:undefined,done:true}

               }

           }

       }
```

```
 // const arr=[1,2,3];

    // const set=new Set(["a","b"]);

    

    // //array map string set具有Symbol.iterator属性

    // let itArr=arr[Symbol.iterator]();

    // let itSet=set[Symbol.iterator]();



    // console.log(itArr.next());



    // console.log(itSet.next())

    // console.log(itSet.next())

    // console.log(itSet.next())
```

```
// //set map 数据遍历

    // let s=new Set(['a',"b","c"]);



    // for(let items of s){

    //     console.log(items);//abc

    // }



    // //map数据

    // let map =new Map().set("a",1).set("b",2);

    // // map{a:1,b:2}

    // // set{}

    // for(let pair of map){

    //     console.log(pair);//[a:1][b,2]

    // }
```

## generator

```
function* g(){

        yield "a";

        yield "b";

        yield "c";

        return "ending";

        yield "d";

    }

    //console.log(g());//返回一个对象

    //iterator接口的时候 next()

    let gen=g();

    console.log(gen.next());

    console.log(gen.next());

    console.log(gen.next());

    console.log(gen.next());

    console.log(gen.next());
```

```
//generator函数next互不干扰



    function* a(){

        let o=1;

        yield o++;// ++o o++ 2

        yield o++;

        yield o++;

    }

    //每个迭代器之间互不干扰

    let a1=a();

    console.log(a1.next());//1

    let a2=a();

    console.log(a1.next());//2

    console.log(a2.next())//1

    console.log(a1.next());//3
```

```
//嵌套

    function* b(){

        yield 'a';

        yield "b";

    }

    function* c(){

        yield 'x';

        yield* b();

        yield "y";

    }

    let c1=c();

    console.log(c1.next());//x

    console.log(c1.next());//a  y



    console.log(c1.next());



    console.log(c1.next());
```

```
//案例  读取   a.html  b.html   c.html



    function read(url){

        $.get(url, function(response){

            it.next(response);

        })

    }

    function* d(){

        console.log(yield read("./a.html"));

        console.log(yield read("./b.html"));

        console.log(yield read("./c.html"));

    }

    var it=d();

    it.next();//a.html  b.html c.html  
```

```
axios底层原理的封装

<!DOCTYPE html>

<html>

	<head>

		<meta charset="UTF-8">

		<title></title>

	</head>

	<body>

	</body>

	<script type="text/javascript">

		function ajax(method,url,data){

			//{name:"ss"}  name=ss&

			//数据操作  将数据变为  键名=简直& 格式

			function das(data){

				var str = "";

				for(var i in data){

					//i name  data[i] ss

					str += i + "=" + data[i] + "&"

				}

				return str.slice(0,-1)

			}

			return new Promise((res,rej)=>{

				var xhr = new XMLHttpRequest();

				//判断传输方式

				if(method=="get"){

					xhr.open("GET",url+"?"+das(),true);

					xhr.send()

				}else if(method=="post"){

					xhr.open("POST",url,true);

					//请求头...  不会背

					

					xhr.send(das())

				}

				//监听

				xhr.onreadystatechange=function(){

                if(this.readyState==4){

                	

                    if(this.status==200){

                    	//成功回调

                        res(this.responseText)

                    }else{

                        //status

                        let status=this.status;

                        //失败回调

                        rej(status);

                    }

                }

            	}



			})

		}

		//调用函数

		ajax('get','http://localhost:8080/add',{name:"ss",age:22}).then((data)=>{

			console.log(data)

		},(res)=>{

			console.log(res)

		})

	</script>

</html>
```

## class语法糖

es5基础语法

```
//class  语法糖

        // function Point(x,y){

        //     this.x=x;

        //     this.y=y;

        // }

        // //this-->实例对象

        // //原型对象---》空对象

        // Point.prototype.add=function(){

        //     // console.log(this)

        //     return this.x+this.y;

        // }

        // let p=new Point(1,2)

        // console.log(p.add())

        //es5的构造函数   多练 
```

es6基础语法

```
//class语法糖  首字母大写

        // class Point{

        //     //this.x=x

        //     // constructor(){}

        //     //constructor  默认指向Point

        //     constructor(x,y){

        //         this.x=x;

        //         this.y=y;

        //     }

        //     add(){

        //         return this.x+this.y

        //     }

        // }



        //创建实例的时候，跟es5一毛一样

        //new 关键的5个过程

        // let p=new Point(1,2);

        // console.log(p.add())
```

//es6改变原型对象  完成继承 

```


    //    let obj={

    //        f1(){

    //            console.log("f1")

    //        }

    //    }



    //    class Point{

    //         constructor(){

    //             //改变指向，完成原型对象的更改

    //             return obj;

    //         }

    //         f2(){

    //             console.log("f2")

    //         }

    //    }

    //    let p=new Point();

    //    p.f1();

    //    p.f2();
```

 //不存在变量提升

​       

```
       class Content{

           //属性和方法

           constructor(name,age){

               this.name=name;

               this.age=age

           }

           //方法

           toHappy(){

               console.log("哈哈")

           }

       }



    //    function A(){}

    //    A.prototype.toSad=()=>{

    //        console.log("呜呜呜")

    //    }



       let c=new Content();

       c.toHappy()

       //语法糖在原型对象追加方法跟es5是一样的

       Content.prototype.toSad=()=>{

           console.log("呜呜")

       }
 c.toSad()
```

  

## class语法糖中this指向   

//class语法糖this指向

```
        // class Logger{

        //     //es5的

        //     printName(name="there"){

        //         this.print(`

        //             hello ${name}

        //         `)

        //     }

        //     print(text){

        //         console.log(text)

        //     }

        // }
// let l=new Logger();
        // l.printName();
        // //解构赋值   对象
        // let {printName}=l;//对象
        // // let printName=()=>{

        // // }
        // printName();//this--->win
```

解决方案 2种

 //第一种方式  使用bind关键字  绑定this指向

```
   class Logger{

            constructor(){

                //绑定this指向为  class Logger

                this.printName=this.printName.bind(this)

            }

            printName(name="there"){

                this.print(`

                    hello ${name}

                `)

            }

            print(text){

                console.log(text)

            }

        }

        



        // let l=new Logger();

        // l.printName();

        // //解构赋值   对象

        // let {printName}=l;//对象

        // // let printName=()=>{



        // // }

        printName();//this--->win
```

 //使用箭头函数

```
 

        class Logger{

            constructor(){

                this.printName=()=>{

                    //console.log(this)

                    return this//--->window class

                }

            }

        }



        let l=new Logger();

        l.printName();

        let {printName}=l;//对象

        //this--->win----->class 语法糖

        console.log(l.printName()===l);//true
```

## es6继承

```
class Run{

            p(){

                console.log("p")

            }

        }



        //子语法糖  

        class Man extends Run{

            constructor(props){

                super(props);  //new Run

            }

        }

        //React.Component

        let m=new Man()

        m.p();
```

//es6的继承  

​      

```
  // super()  

        

        class A{

            constructor(name){

                this.name=name;

            }

            say(){

                console.log("hello world")

            }

        }

        class B extends A{

            constructor(){

                super()   //  a=new  A  a.name  say()   当作函数去调用  

            }

        }
```

## aysan函数

```
<!DOCTYPE html>

<html lang="en">



<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <meta http-equiv="X-UA-Compatible" content="ie=edge">

​    <title>Document</title>

</head>



<body>



</body>

<script src="../day03/jquery-3.3.1.min.js" type="text/javascript" charset="utf-8"></script>

<script>

​    function ifRand() {

​        let p = new Promise((resolve, reject) => {

​            //生成一个0-10的随机数

​            let num = Math.floor(Math.random() * 11);

​            console.log(num);

​            if (num >= 5) {

​                resolve("成功");

​            } else {

​                reject("失败的原因是数字小于5");

​            }

​        })

​        return p;

​    }



​    ifRand().finally(function (data) {

​        console.log(data)

​    })



​    // ifRand().then(function(data){

​    //     console.log(data)

​    // },function(reason){

​    //     console.log(reason);

​    // })



​    let str="hello"

​    console.log(str.slice(0,-1))



​    async function read(){

​        let step1=await new Promise((resolve,reject)=>{

​            $.get("./a.html", function(data) {

​                console.log(data)

​                resolve();

​            })

​        })

​        console.log(step1)

​        let step2=await new Promise((resolve,reject)=>{

​            $.get("./b.html", function(data) {

​                console.log(data)

​                resolve();

​            })

​        })

​        console.log(step2)

​        let step3=await new Promise((resolve,reject)=>{

​            $.get("./c.html", function(data) {

​                console.log(data)

​                resolve();

​            })

​        })

​        console.log(step3)

​        return [step1,step2,step3]

​    }

​    read()

</script>



</html>
```

## module

### 概述

历史上，JavaScript 一直没有模块（module）体系，无法将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。其他语言都有这项功能，比如 Ruby 的`require`、Python 的`import`，甚至就连 CSS 都有`@import`，但是 JavaScript 任何这方面的支持都没有，这对开发大型的、复杂的项目形成了巨大障碍。

在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务器，后者用于浏览器。ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

```javascript
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

ES6 模块不是对象，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入。

```javascript
import { stat, exists, readFile } from 'fs';
```

上面代码的实质是从`fs`模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。

## export 命令

模块功能主要由两个命令构成：`export`和`import`。`export`命令用于规定模块的对外接口，`import`命令用于输入其他模块提供的功能。

一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用`export`关键字输出该变量。下面是一个 JS 文件，里面使用`export`命令输出变量。

```javascript
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```

`export`的写法，除了像上面这样，还有另外一种。

```javascript
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };
```

`export`命令除了输出变量，还可以输出函数或类（class）。

```javascript
export function multiply(x, y) {
  return x * y;
};
```

通常情况下，`export`输出的变量就是本来的名字，但是可以使用`as`关键字重命名。

```javascript
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

需要特别注意的是，`export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。



```javascript
// 报错
export 1;

// 报错
var m = 1;
export m;
```

上面两种写法都会报错，因为没有提供对外的接口。第一种写法直接输出 1，第二种写法通过变量`m`，还是直接输出 1。`1`只是一个值，不是接口。正确的写法是下面这样。

```javascript
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```

`export`命令可以出现在模块的任何位置，只要处于模块顶层就可以。

```javascript
function foo() {
  export default 'bar' // SyntaxError
}
foo()
```

上面代码中，`export`语句放在函数之中，结果报错。

## import 命令

使用`export`命令定义了模块的对外接口以后，其他 JS 文件就可以通过`import`命令加载这个模块。

```javascript
import { firstName, lastName, year } from './profile.js';

function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}
```

如果想为输入的变量重新取一个名字，`import`命令要使用`as`关键字，将输入的变量重命名。

```javascript
import { lastName as surname } from './profile.js';
```

`import`命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。

```javascript
import {a} from './xxx.js'

a = {}; // Syntax Error : 'a' is read-only;
```

上面代码中，脚本加载了变量`a`，对其重新赋值就会报错，因为`a`是一个只读的接口。但是，如果`a`是一个对象，改写`a`的属性是允许的

```javascript
import {a} from './xxx.js'

a.foo = 'hello'; // 合法操作
```

## export default 命令

上面代码的两组写法，第一组是使用`export default`时，对应的`import`语句不需要使用大括号；第二组是不使用`export default`时，对应的`import`语句需要使用大括号。

`export default`命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此`export default`命令只能使用一次。所以，import命令后面才不用加大括号，因为只可能唯一对应`export default`命令。

## proxy

```
let handle={

            get(){

                return "哈哈哈"

            }

        }

        

        //要访问的对象  target{}

        //拦截器       handle

        let proxy=new Proxy({



        },handle)

        console.log(proxy.name);

        console.log(proxy.age);
```

## react

```
 //react简介

    {

        //浏览器端 dom树 

        //在去追加dom

        //let p=document.createElement("p");

        //react 创建dom  target 属性attr  子元素/内容

        let h1=React.createElement("h1",{id:"myTitle"},"这就是react创建dom元素");

        //原生js渲染

        // let app=document.querySelector("#app");

        // app.appendChild(p);  

        //react渲染dom

        ReactDOM.render(h1,document.querySelector("#app"));



        // warining:警告-->正确的写法

        // 原生创建子元素

        p.innerHTML=`

            <span>今天小雨转晴</span>

        `

    }
```

```
{

        // react创建子元素

        //第三个参数中接收子元素或者是内容

        //let h1=React.createElement("h1",{className:"myTitle",id:"my"},"<span>哇哈哈矿泉水</span>")

        let h1=React.createElement("h1",{className:"myTitle"},

            //创建一个p标签   子元素

            React.createElement("p",{id:"p1"},"哇哈哈矿泉水")

        )

        //渲染

        ReactDOM.render(h1,document.querySelector("#app"))

    }
```

```
 {

​        //n次嵌套

​        //解构赋值

​        let {createElement:c}=React;

​        let ul=c("ul",null,

​            c("li",null,"一级菜单"),

​            c("ul",null,

​                c("li",null,"二级菜单"),

​                c("ul",null,

​                    c("li",null,"三级菜单"),

​                    c("li",null,"三级菜单"),

​                ),

​                c("li",null,"二级菜单"),



​                c("ul",null,

​                    c("li",null,"三级菜单"),

​                    c("li",null,"三级菜单"),

​                )

​            )

​        )

​        // jsx

​        ReactDOM.render(ul,document.querySelector("#app"))

​    }
```

