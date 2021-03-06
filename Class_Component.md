###### tags: `React`
# Class Component

- Same Concept as Angualr's component

## Declaration of a class component
```jsx
class Class_Name{
    constructor(){
        super() 
    }
    
    method_1(){
     
    }
    method_2(){
      //...
    }
    
    // other methods 
}
```


## extend Component 

App.js uses a React Component
```jsx
// import react comonent
import React, { Component } from 'react'; 
class App extends Component{ // to extend Component
    render(){
    return(
      <div>
        helloWorld
      </div>
    );
    }
}
export default App;
```
- 在React component渲染DOM前/後其實是有一連串的流程 (a.k.a生命週期)，render()是在渲染前最後一個階段(有個例外，但那個例外很少用)
- **`render()`只是渲染前最後一個階段，元件還沒有真的渲染到DOM上。所以不要在`render()`中操作有關`document.methods`**

## Declaration of member data(field) by using `this` in component
```jsx
class ClassName{
    constructor(){
        this.field = value;
    }
}
```

we can also define a member inside the method/function (like getter and setter method)
```jsx
function func(val2){
    this.field_in_Func = val2;
}
```
- if `this.field_in_Func` haven't declared in the `constructor` then it will be invoked as `func` is called


## props in Constructor

```jsx
import React, { Component } from 'react';
class App extends Component{

    // To use props
    constructor(props) {
        super(props);
    }
export default App;
```

使用時和使用js的class的member data一樣，需要加上this
```jsx 
    render(){
        return(
          <button onClick={this.props.handleClick}>
              {this.props.name}
          </button>
        );
    }
```



## Call functions/methods in the same class Scope

To call the method in same class's scope , we need to assign like this 
```jsx
this.memberFunctionName = this.memberFunctionName.bind(this)
```
- 這是因為javascript的`this`在class的member function中是指向`undefined`


#### When we are going to use such method?
- for example a constructor to call its own member function ...


### Example 

Let's modify the above two files `index.js` and `App.js` 
 
```jsx
// A index.js before modifying
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

const changeName=(newName)=>{ 
    // name is not a state member data
    name=newName;
}

var name="oldName";

ReactDOM.render(
    <div>
        // changeName will not function 
        <App name={name} handleClick={changeName}/>
    </div>,
    document.getElementById('root')
);
```


With component concept we can now put `changeName()` as member function in `App.js` instead of placing it in `index.js`
```jsx
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
    <div>
        <App/>
    </div>,
    document.getElementById('root')
);
```

```jsx
// App.js
import React, { Component } from 'react';
class App extends Component{
    constructor(props) { 
        super(props);
        this.name="old_Name";
        
        // To call itself
        this.changeName=this.changeName.bind(this); 
}

changeName(newName){
   this.name=newName;
   console.log("hey");
 }

render(){
    return(
      // when we press the button then name will be changed
      //    from old_Name to newName
      <button onClick={this.changeName}>{this.name} </button>
    );
}
```
- We have to use `this.` to call the member data(methods) from `Component`
- `App.js` just like a component can be place to different js files (increase the Reusability)

## A function component without state member data 

Here comes the problem (e.g the above example), when a button in the webpage have been activated/pressed by user (click event) the name will not change

- React(**update**) component only when:
    > 1. props's fields change
    > 2. state's fields change
    >>  These have ReactDOM re-render the component's update phase，and update the view(web page)

We can use [Using State Member to Handel the Problem](https://hackmd.io/cbqcgJS8TLupSn13aslpvw)
