###### tags: `React`
# State

State是class component的預設好會去檢查的一個特殊的member field。

所以當state被改變時，會進入re-render的update程序，更新畫面。

## Declaration and Definition of a state member data in class component
```jsx
constructor(props) {
    super(props);
    this.state={ 
        stateA: valA, 
        stateB: valB, 
        // .. son on
    };
}
```


### Call state member data

```jsx
this.state.stateName
```

For example :: use `state` to control progress-bar's width

```jsx
import React, { Component } from 'react';
class App extends Component{
  constructor(props) {
    super(props);
    this.state={
      percent:"30%" 
    }
  }

    render(){
        return(
          <div>
            <div className="progress-back" style={{backgroundColor:"rgba(0,0,0,0.2)",width:"200px",height:"7px",borderRadius:"10px"}}>
              
              // call a state percent this.state.percet
              <div className="progress-bar" style={{backgroundColor:"#fe5196",width:this.state.percent,height:"100%",borderRadius:"10px"}}>
              </div>
              
            </div>
          </div>
        );
    }
}
export default App;
```


## Update State members via `setState()`

The state type members are `read-only`

So if we intend to change/update a state member data like this way
```jsx
this.state.stateNmae  =  val2 ;
```
- It will cause error.


Instead we use setter method `setState()` provided by React to deal with it

For example :: Change the progress-bar's percent(width) from 30% to 70% using event binding
```jsx 
import React, { Component } from 'react';
class App extends Component{
  constructor(props) {
    super(props);
    this.state={
      percent:"30%"
    }
    this.changePercent=this.changePercent.bind(this); //綁定changePercent
  }

  changePercent(){ 
    this.setState({percent:"70%"})
  }
    render(){
        return(
          <div>
            <div className="progress-back" style={{backgroundColor:"rgba(0,0,0,0.2)",width:"200px",height:"7px",borderRadius:"10px"}}>
              <div className="progress-bar" style={{backgroundColor:"#fe5196",width:this.state.percent,height:"100%",borderRadius:"10px"}}></div>
            </div>
            // if user clicks the button 
            //  it will change the progress-bar from 30% to 70%
            <button onClick={this.changePercent}>
                To 70% 
            </button>
          </div>
        );
    }
}
export default App;
```


### More Details of `setState()`

We don't really need to initialize/define a state member data in constructor
```jsx
this.state={
    percent: 20,
    // create a state member called mounted instead create it in constructor
    mounted: false
}
```
- 當我們使用`this.setState({percent: 70})`時，mounted並不會從state中被移除。

## To udpate/create a new State Member

```jsx
this.state={
    percent: 20,
    mounted: false
}
``` 

設定一個狀態(STATE)表示在改變percent為40的同時，創造(CREATE A NEW STATE)叫做`counter`的`state`
```jsx
this.setState({
    percent:40,
    counter:0
});
```

so after percent's state increases to 40 the state will look like
```jsx
this.state={
    percent: 40,
    mounted: false,
    counter: 0
}
```

### Delete the State member

```jsx
this.setState({
    stateName: undefined
});
```
- this will kick out `stateName` 

### Declaration only 

使用變數設定state值時，可以只寫`this.setState({變數名稱})`

當寫成`this.setState({變數名稱})`時，setState會自動去找state中有沒有和該變數同名字的`member/field`。   
如果有，就會把它設定為變數值。如果沒有，就會自動在state中建立同名字的member data。

```jsx
let counter=5;
this.setState({counter}); // it will automatically set counter as 5
```

### Modify One of fields in the State Object

For an object 
```jsx
this.state={
    styleData:{
        width: "30%",
        height: "50%"
    }
}
```

if we change one of fields (for example `width`) in the object
```jsx
// this will elimite height
this.setState({ styleData:{width:"70%"} });
```
- state裡面的styleData並不會保留height屬性，而是直接變成只有`width:"70%"`的物件。

To handle such problem
```jsx 
this.setState({ 
    styleData:{
        width: "70%",
        height: this.state.styleData.height
    } 
});
```

## second parameter of setState method

```jsx
this.setState((state, props) => {
     /* 第一個參數函式 */
  return {新的state};
},()=>{
     /* 第二個參數函式，state改變後執行 */
});
```

for example
```jsx
this.state={
    percent: 20
}
this.setState(
    {percent: 70},
    ()=>{
        // print state percent 70 in the console 
        console.log(this.state.percent);
    }
)
```

## A react hook `useState` method for function component

To declare a state in function component by using userState method instead of the Class Component, we call it as `react hook`

For `useState` we pass initialize value for a desire state and it returns an array contains `{stateMember, setStateMethod}`

For example :: In function component
```jsx
dataType[stateMember, setStateMethod] = useState(value)

const [percent, changePercent] = useState("20%");

// to update a state to 70%
changePercent("70%");
```

This is actually same as we declare in class Component
```jsx=
const changePercent = (value) => {
    this.setState({percent: value})
}
```

### Compare the difference btw Class/Function Component 

A Class Component `App.js`
```jsx
import React, { Component } from 'react';
class App extends Component{
  constructor(props) {
    super(props);
    
    // initial state
    this.state={
      percent:"30%"
    }
    
    this.changePercent=this.changePercent.bind(this);
  }

  changePercent(){ 
    this.setState({percent:"70%"})
  }

    render(){
        return(
          <div>
            <div className="progress-back" style={{backgroundColor:"rgba(0,0,0,0.2)",width:"200px",height:"7px",borderRadius:"10px"}}>
              
              <div className="progress-bar" style={{backgroundColor:"#fe5196",width:this.state.percent,height:"100%",borderRadius:"10px"}}>
              </div>
            
            </div>
            
            <button onClick={this.changePercent}>
                To 70% 
            </button>
          </div>
        );
    }
}
export default App;
```

A Function Component `progress.js`
```jsx
import React, { useState } from 'react';

const Progress=()=>{
    const [percent, changePercent] = useState("20%");  
    
    return(
    // paste render()'s content in App.js 
      <div>
        <div className="progress-back" style={{backgroundColor:"rgba(0,0,0,0.2)",width:"200px",height:"7px",borderRadius:"10px"}}>
            <div className="progress-bar" style={{backgroundColor:"#fe5196",width:percent,height:"100%",borderRadius:"10px"}}></div>
            </div>
        <button onClick={()=>{changePercent("70%")}}>
            To 70
        </button>
      </div>
    );
}
export default Progress;
```

Export function component to `index.js`
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import Progress from './Progress'
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
    <div>
        <Progress/>
    </div>,
    document.getElementById('root')
);

serviceWorker.unregister();
``` 


## `setState` forbids to contain a loop body in function component

`setState`(和其他的React hook)不能用在function component中的迴圈、if-else、nest function(在function scope中宣告的function)被定義使用。
精確的說法是你不能在這些地方去定義產生React hook。(例如，宣告由變數和函式並從useState取得)

## React Hook

對`useState`而言，它是依照順序去分辨每一個hook

#### At First render

```jsx
useState(1)           
useState('2')        
useState(true) 
```

React will 
1. Create hook，mark as first one，invoke a state member assigned as 1
2. Create hook，mark as second one，invoke a state member assigned as '2'
3. Create hook，mark as third one，invoke a state member assigned as true

#### At Re-render Stage

```jsx
useState(0)           
useState('2')        
useState(true) 
```

React will
1. find out first hook useState(1)
2. find out second hook useState('2')
3. find out third hook useState(true)

