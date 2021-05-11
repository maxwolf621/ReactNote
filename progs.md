###### tags: `React`
#  props and function binding
[TOC]

# progs
:::info
Using props to control the component's attributes (just like html tag's attributes `style`, `value` and `onclick` ... etc) as a object 
:::

For example there is react's component 
```jsx
<App version="4" data="none"/>
```
App的props包含了version、data，也就是對App來說，它接到一個像這樣結構的Object參數:
```json
props:{
    version: "4",
    data: "none"
}
```
- We can it props binding for such way


## Example 
Using a props to display the button's name
```jsx
ReactDOM.render( 
    <div>
        // add a name attribute for component App
        <App name="我的名字"/> 
    </div>,
    document.getElementById('root')
);
```

In component app.js 
```jsx
function App(props){
    return(
         <button> {props.name} </button>
    );
}
```

![](https://i.imgur.com/nM9CUEH.png)


## DataType of props

以上是props的基礎使用方法。接下來講個前面提過但還是很容易踩到的坑: 
- JSX語法中的資料型態問題。例如: 在下面的程式碼中，App接到的number和getData都是字串，並不是整數和布林值。

```jsx
<App number="87" getData="true"/>
```
必須要加上`{}`才會接到正確的js資料型態，使用變數時也要這樣。
```jsx z
<App number={87} getData={true}/>
```

We cant assign a props.variable a value
```
this.props.variable= vlaue;
```
- this will cause error 

# Function Binding


#### Father and Son
```bash
index.js
    |____ App.js
```

-  Use Component by importing Component 
    > 稱為父 (如: index.js)
-  To be used 
    > 稱為子元件 (如: App.js)

**在這邊，我們會試著在父(index.js)定義一個改變父的函式，綁定在子元件(App.js)的props，並透過子元件觸發。**

To use a function binding in index.js
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

const printMessage=()=>{
    document.getElementById('show-area').innerHTML="我被按到了";
}

ReactDOM.render(
    <div>
        // binding it
        <App name="我的名字" handleClick={printMessage}/>
        <div id="show-area"></div>
    </div>,
    document.getElementById('root')
);
```
In App.js `handleClick={printMessage}` is binding to button's `onClick` via `props`
```jsx
function App(props){
  return(
     // { } means using jsx
    <button onClick={props.handleClick}>{props.name}</button>
  );
}
```


# progs children

A react component can be used like 
```jsx=
//** 
//    common way
<react_component/>

// ** 
//     nested way
<react_component> (THINGS) </react_component>
```
- We name `(THINGS)` as children。

So this header is going to talk about how to handle children inside `<react_component\>`
- children是props之一，所以當使用的children改變時，畫面也會重繪。

### Usage
- Using state or react-router-dom to content of progs.children，
- For component design，vis progs.children it divides construction into mltuiple layers (e.g. boostrap中的List & ListItem)



### Example

Let's Add some word btw `<App> ... </App>`
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
    <div>
        <App> Im_A_Children </App>
    </div>,
    document.getElementById('root')
);

```

在App.js函式中button標籤內使用children。
因為children是props之一，所以使用方法為props.children。
```jsx 
function App(props){
  return(
       <button> {props.children} </button> 
  );
}
export default App;
```


Component Layout.js
```jsx 
import React from 'react';

const Layout=(props)=>{
    const navStyle={
        position:"fixed",
        top:"0",
        left:"0",
        width:"100%",
        height:"43px",
        backgroundColor:"rgb(66, 103, 178)",
        display:"flex",
        alignItems:"center"
    };
    const iconStyle={
        marginLeft:"10%",
        height:"25px",
        width:"25px",
        borderRadius:"1px",
        backgroundColor:"white"
    };
    const inputStyle={
        marginLeft:"5px",
        padding:"0px 7px",
        height:"25px",
        width:"28%",
        borderRadius:"2px",
        border:"none",
        backgroundImage:"url('https://cdn1.iconfinder.com/data/icons/hawcons/32/698627-icon-111-search-512.png')",
        backgroundPosition:"97% 50%",
        backgroundSize:"auto 80%",
        backgroundRepeat:"no-repeat"
    };
    return(
      <div>
          <div className="nav-bar" style={navStyle}>
            <div  className="icon" style={iconStyle}>
                <img style={{height:"120%"}} src="picture.png" alt="icon"/>
            </div>
            <input placeholder="搜尋" style={inputStyle}/>
          </div>
          <div className="index-container" style={{marginTop:"43px"}}>
              {props.children}
          </div>
      </div>
    );
}
export default Layout;
```
index.js:
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import Layout from "./Layout";
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
    <div>
        <Layout>
            <App>在index.js中設定文字</App>
        </Layout>
    </div>,
    document.getElementById('root')
);
serviceWorker.unregister();
```
