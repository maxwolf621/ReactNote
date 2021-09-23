###### tags: `React`
#  props and function binding
[TOC]

# progs

We use `props` to control the value of component's attributes (like `@input`, `@output` in angular) ...

For example there is react's component    
```jsx
<App version="4" data="none"/>
```

App的`props`包含了`version` and `data`，也就是對App來說，它接到一個像這樣結構的Object參數:
```console
props:{
    version: "4",
    data: "none"
}
```

## Using a props to display the button's name
 
```jsx
ReactDOM.render( 
    <div>
        // add a name attribute for component App
        <App name="我的名字"/> 
    </div>,
    document.getElementById('root')
);
```

In `component app.js`
```jsx
function App(props){
    return(
         <button> {props.name} </button>
    );
}
```

![](https://i.imgur.com/nM9CUEH.png)


## props內部的資料型態


using props to pass different data type of parameters 

```jsx
// these value are string type
<App number="87" getData="true"/>

// if we need to sepcify the data type then we use `{}`
<App number={87} getData={true}/>
```

We cant assign a `props.variable` a value
```
this.props.variable= vlaue;
```

# Function Binding Base and Derived

```bash
index.js
    |____ App.js
```
-  Use Component by importing Component 
    > 稱為父 (如: `index.js`)
-  To be used 
    > 稱為子元件 (如: `App.js`)

**在這邊，我們會試著在父(index.js)定義一個改變父的函式，綁定在子元件(App.js)的props，並透過子元件觸發。**

To use a function binding in `index.js`
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
        // pass the props to child component
        <App name="我的名字" handleClick={printMessage}/>
        <div id="show-area"></div>
    </div>,
    document.getElementById('root')
);
```

In App.js `handleClick={printMessage}` is binding to button's `onClick` via `props` , `{name="我的名子"}` binds to button's name
```jsx
function App(props){
  return(
     // { } means using jsx
    <button onClick={props.handleClick}>{props.name}</button>
  );
}
```


## progs children

A react component can be used like 
```js
//    common way
<react_component/>

//     nested way
<react_component> (THINGS) </react_component>
```
- We name `(THINGS)` as children。

So this header is going to talk about how to handle children inside `<react_component\>`
- `children`是props之一，所以當使用的children改變時，畫面也會reload  
- Using `state` or `react-router-dom` as content of `progs.children`
- For component design，`progs.children` divides construction into mltuiple layers (e.g. `boostrap`中的`List` & `ListItem`)


put value that will pass to `progs.children` btw `<App> progs.children's value </App>`
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
    <div>
        // <html_ELEMENT> pass the value TO_CHILD to child component <html_ELEMENT>
        <App> TO_CHILD </App>
    </div>,
    document.getElementById('root')
);
```



Example 
`index.js`
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
            <App> TO_PASS_VALUE_TO_LAYOUT </App>
        </Layout>
    </div>,
    document.getElementById('root')
);
serviceWorker.unregister();
```

In `App.js`
```jsx 
function App(props){
  return(
       <button> {props.children} </button> 
  );
}
export default App;
```

Component `Layout.js`
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
              {props.children}  // it will display TO_PASS_VALUE_TO_LAYOUT
          </div>
          
      </div>
    );
}
export default Layout;
```
