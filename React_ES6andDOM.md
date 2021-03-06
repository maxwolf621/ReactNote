###### tags: `React`
# ES6 and DOM

## DOM
Document Object Model, DOM is described these files `HTML`、`XML` and `SVG`  

If we want to get an DOM's elements in react
```jsx
document.getElementById
```

## ES6 

React的結構非常仰賴ES6的class。

### Declarations

Since ES6 give me us the options to `let` and `const` instead of initializing the variable only as global variable via `var`
```jsx
// local variable
let local_1;
let local_1 = value;
// constant variable
const readOnly_2 = value;
```

### Arrow Function

Declaration of Function Before ES6 
```jsx
function fun_name (parameters){
    // function body ...
}

/* OR */

var function_name = function (parameters){
    // function body ...
}
```

Before ES6 we assign a value to a fields with `this.field` always

```jsx
DataType function = (parameters) => {
    // parameter operation
}

/* for example */
var testFunction = (A, B)=>{
    return A+B;
}
```


### spread operator `...arg`

Allow a function has different numbers of parameters, that is parameters are dependent on user-input
```javascript
const getAll=(...arg)=>{
    // print as passed parameters as array
    console.log(arg)
    // print args
    console.log(...arg)
}

// getAll's parameters can be passed with 
//     different numbers of parameters as the following
getAll("A","B","C",1);
getAll("B",1);
```


### Promise

```jsx
宣告型態 宣告名稱 = new Promise((resolve, reject)=>{
    定義要先做什麼事情
    resolve(參數);
})

宣告名稱
.then((參數，由resolve丟出)=>{ 
    定義要後做什麼事情 
})
.catch((錯誤)=>{ 
    先做的事情出現錯誤時怎麼處理 
}))
```

### 解構賦值 (Alias)


For example
```jsx
/* For Array */
const arr=[ "apple" , "banana" ];

// this mean 
// a is arr[0]  , b is arr[1]
const [ a , b ] = arr;

console.log("a is "+a); // a is apple
console.log("b is "+b); // b is banana

/* For Object */

const obj={ fruitOne: "apple", fruitTwo: "banana" };
const { fruitOne: a , fruitTwo: b  } = obj;

console.log("a is "+a); // a is apple
console.log("b is "+b); // b is banana
```


### Module via `exprot`

ES6中，我們可以使用`export`把js函式、變數、物件打包成模組，然後在其他js檔引入使用

```jsx
export const helloWorld=()=>{
    console.log("hello world!");
}

export const msg="hello world";
```

if `index.js` want to use function/variable inside module `hello.js`
```jsx
/*import + {Method_A, Method_B, ... } + from + path*/
import {helloWorld, msg} from "./hello.js";

helloWorld();
console.log(msg);

/* Using alias */
import {helloWorld as myFun , msg as myMsg} from "./hello.js";
myFun();
console.log(myMsg);
```

## export default component/method/variable

with `default` we can export a class to be used by others

```jsx
const helloWorld=()=>{
    console.log("hello world!");
}

export const msg="hello world";

// helloWord is a class's method
export default {helloWorld};
```

在別的js做`import`時，我們只要給export default的物件一個名稱(Alias)就能使用。
```
import Instance_for_default_class, {Non_default_members_in_Module} from "Where_Module_Locates";
```

for example 
```jsx
import hello,{msg} from "./hello.js";

// declare an export-default object instance as hello 
hello.helloWorld();
console.log(msg);
```



