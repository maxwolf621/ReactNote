###### tags: `React`
# How React renders the DOM  


We use `ReactDOM.render( ... )` to render the HTML webpage
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />

    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <!-- here is what react will do its job -->
    <div id="root"></div>
  </body>
</html>
```

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
    <h1> Hello world!</h1>, 
    /* find <div id=root> tag 
     *  render it as <h1> Hello world!</h1>
     * */  
    document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
serviceWorker.unregister();
```


## JSX

we can encapsulate html syntax inside a react method and use `ReactDom.render` to call that method

for example 
```jsx
const testFunction =()=> {
    return( <button>Im_A_Button</button> );
}

ReactDOM.render(
    testFunction(),
    document.getElementById('root')
);

```

if we want to contains multiple html syntax inside the method
we should use `<div> ... </div>`
```jsx
const testFunction =()=> {
    return( 
        <div>
            <button> I'm a button </button>
            <h1> I'm Header </h1>
        </div>
    );
}

ReactDOM.render(
    testFunction(),
    document.getElementById('root')
);
```



## `className`

If we want to render `<h1 class = "title" > Hello, world! </h1> `
by react , we use `className` instead of `class`
```javascript=
ReactDOM.render(
    <h1 className = "title" > Hello, world! </h1>,
    document.getElementById('root')
);
```

## `{function}` and `{fields}`

Bind JSX with html by using `{}`

```jsx
const multiButton=()=>{
    var output=[];
    for(let i=0;i<4;++i)
        output.push(<button>我是第{i}個按鍵</button>)
    return output;
}
    
ReactDOM.render(
    <div>
        { multiButton() }
    </div>,
    document.getElementById('root')
);
```

```javascript=
const styleArgument = { fontSize: '100px', color: 'red' };

ReactDOM.render(
    <h1 style={ styleArgument } > Hello, world! </h1>,
    document.getElementById('root')
);
```



### Button

```jsx=
<button value={true} > 是 </button>
<button value > 是 </button>
```

## 點擊(button)和輸入(input/textarea ...)事件

```htmlembedded=
<button value="true" onclick="Invoke_Function(Parameter)">
    是
</button>
```

But in JSX sytnax
```jsx
<button value={true}  onClick={ }>
    press me Dosomething
</button>
```


## 函式綁定
原本我們在綁定函式在button/input/textarea的這種元件時，會這樣寫在html中:
```htmlembedded=
<button value="true" onclick="Invoke_Function(Parameter)">
    是
</button>
```
而在JSX下，函式的綁定有兩種方法:

### 第一種
```
<button value={true}  onClick={ myFunction }>是</button>
```

- 函式的綁定和綁定資料一樣是使用`{函式名稱}`，在這種綁定方法下，只能綁一個函式，然而在輸入類的元件上有一些比較不一樣的地方。當button/input/textarea這類元件的互動事件(按下、輸入、選擇......)觸發時，**函式只會接收到一個event類別的參數，並不能傳遞其他參數，如果我們要取得元素的value，就必須要透過原生DOM的`event.target.value`來取得**

例如 
```jsx
// index.js

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

const getValue=(event)=>{
    console.log(event.target.value)
}


ReactDOM.render(
    <div>
        <button value={true} onClick={getValue}>按下以取得DOM的value </button>
    </div>,
    document.getElementById('root')
);
```

## Arrow Function `;`

```jsx
<button value="true"  onClick={(e)=>{ myFunction1();myFunction2()} }>
    是
</button>
```
在這種綁定方法下，就是以`js`語法定義一個新函式，想傳什麼參數、使用幾個函式、做什麼運算都可以。缺點是很容易讓版面看起來很亂，而且因為是在render創造一個新函式，每次渲染時都會創造一次，會影響效能

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

const getValue=(value)=>{
    console.log(value)
}


ReactDOM.render(
    <div>
        <button value={true} onClick={(event)=>{getValue(event.target.value)}}>
            Press Button Get value 
        </button>
    </div>,
    document.getElementById('root')
);
```
- 要注意的是，在這邊因為在綁定函式時已經把`event.target.value`放到參數，getValue函式接到的就是value的值，不用再去存取接到參數的target value