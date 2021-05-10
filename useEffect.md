# Life Cycle of React Hook of Function Component

To use Life Cycle in a Function Component via React Hook

userEffect = componentDidMount、componentWillUnmount and componentDidUpdate
> React hook把這三者整合起來，變成了useEffect。

## React hook兩大守則
1. 只能在最外層scope宣告
2. 只能在function component或custom hook中使用

## useEffect基本語法
useEffect接收兩個參數，
1. 第一個是一個函式，定義componentDidMount或componentDidUpdate要做什麼事，此函式的回傳值也要是一個函式，表示componentWillUnmount 要做什麼事。
2. 第二個是一個array，裡面是定義當哪些變數被改變時，這個useEffect要重新被觸發。有點像是過去我們在componentDidUpdate寫prevState!=this.state這種感覺。



### useEffect替代純componentDidMount
第二個參數為空array時，代表除了第一次以外，接下來每次re-render時，沒有任何東西的改變可以重新觸發useEffect，所以就等同於componentDidMount。
```javascript
useEffect(() => {
    /* 下面是 componentDidMount */
    
    
    /* 上面是 componentDidMount */
    
}, []); 
/* 第二個參數是用來限定當哪些變數被改變時useEffect要觸發 */
```


### useEffect替代componentWillUnmount
componentWillUnmount就是useEffect第一個用來當參數的函式的return值。

以下是componentDidMount和componentWillUnmount的集合體。
```javascript
useEffect(() => {
    /* 下面是 componentDidMount*/
    
    
    /* 上面是 componentDidMount */
    
    return (() => {
      /* 下面是 componentWillUnmount */
      
      
      /* 上面是 componentWillUnmount */
    });
    
}, []); 
/*第二個參數是用來限定當哪些變數被改變時useEffect要觸發 */
```


### useEffect as componentDidUpdate

但實際上我觀察到在第二個參數array不為空下，第一次render還是會執行effect。查了一下官方文件，精確的說法是第二個參數是用來限制哪些參數在re-render時如果被改變，可以重新觸發useEffect，也就是第一次render無論如何都會執行。(「可以重新觸發useEffect」這句話用英文看會更精確，應該是recreate an effect)。

如果想要實現componentDidUpdate，應該要搭配另外一個React hook: useRef。這裡先不解釋它，可以當作是產生一個「被改變後不會觸發re-render」變數的hook。

也就是我們用useRef去產生一個紀錄是否完成第一次渲染的變數，初始值預設會給false，第一次執行effect後改為true。

也就是嚴格實現componentDidUpdate的語法為:
```javascript
const mounted=useRef();
useEffect(()=>{
  if(mounted.current===false){
    mounted.current=true;
    /* 下面是 componentDidMount*/


    /* 上面是 componentDidMount */      
  }
  else{
    /* 下面是componentDidUpdate */


    /* 上面是componentDidUpdate */

  }

  return (()=>{
       /* 下面是 componentWillUnmount */


      /* 上面是 componentWillUnmount */
  })
},[dependencies參數]); /* 第二個參數是用來限定當哪些變數被改變時useEffect要觸發 */
```
如果有第二個非空參數就相當於過去寫的prevState!=this.state和prevProps!=this.props，如果省略，mount==true的if-else scope就是完全等於純componentDidUpdate。記得要引入useRef()。

useEffect替代componentDidMount + componentDidUpdate
當你「省略第二個用來監控的參數array」或是「該array不為空」時，它就等同於componentDidMount + componentDidUpdate的集合體。省略的話就是代表每次re-render時都會觸發useEffect，不省略不為空則代表當re-render時，如果你放在array中的值有被改變，就會觸發useEffect。

```javascript
useEffect(() => {
    /* 下面是 componentDidMount 和  componentDidUpdate */
    
    
    /* 上面是 componentDidMount 和  componentDidUpdate */
    
    return () => {
      /* 下面是 componentWillUnmount */
      
      
      /* 上面是 componentWillUnmount */
    };
    
}, [dependencies參數]); /* 第二個參數是用來限定當哪些變數被改變時useEffect要觸發 */
下面省略了第二個參數，也是componentDidMount 和 componentDidUpdate集合體:

useEffect(() => {
    /* 下面是 componentDidMount 和  componentDidUpdate */
    
    
    /* 上面是 componentDidMount 和  componentDidUpdate */
    
    return () => {
      /* 下面是 componentWillUnmount */
      
      
      /* 上面是 componentWillUnmount */
    };
    
});
```

## useEffect的其他
可以有多個useEffect存在同一function component和custom hook中，所以我們可以針對不同的變數去寫不同的useEffect。



把之前的Baby.js改成function component吧!
```javascript
import React, { useState } from 'react';
const Baby=(props)=>{
    /* 把state變成useState */
    const [isGetData,setGetData]=useState(false);
    const [Mom,setMom]=useState("");
    const [isRightDad,setRightDad]=useState(false);

    /* 把class 的 member function改成function中的function */
    const ajaxSimulator=()=>{
        setTimeout(()=>{
            setGetData(true);
            setMom("小美");
        },3000)
    }
    const checkDad=()=>{
        if(props.dad==="Chang")
            setRightDad(true)
        else
            setRightDad(false)
    }

    if(isRightDad===false)
        return(
            <div>他的媽媽，是誰，干你X事</div>
        );
    else if(isGetData===false)
        return(
            <div id="msg">讀取中</div>
        );
    else
        return(
            <div id="msg">他的媽媽是{Mom}</div>
    );  
}
export default Baby;
```
App.js也順便換成function component，然後加入一個可以換爸爸的功能:
```javascript
import React, { useState } from 'react';
import Baby from './Baby'

const App=()=>{
    const [dad, setDad] = useState("Chang");
    const [born, setBorn] = useState(true);
    
    const changeDad=()=>{
      if(dad==="Chang"){
        setDad("Wang")
      }
      else{
        setDad("Chang")
      }
    }

    const spawnBaby=()=>{
      if(born===true){
        return <Baby dad={dad}/>;
      }
    }

    return(
      <div>
        {spawnBaby()}
        <div id="talk"></div>
        <button onClick={changeDad}>換爸爸!</button>
        <button onClick={()=>{setBorn(!born)}}>{(born===true)?"讓他回去肚子裡":"讓他生"}</button>
      </div>
    );
}
export default App;
```

step 1 : 引入useEffect
`import React, { useState, useEffect } from 'react';`
step 2 : 替代Baby.js中的componentDidMount的ajax部份
**當第二個參數為空array時，這個useEffect就相當於是componentDidMount。**
所以，我們現在讓Baby.js在被創造時，執行花3秒取得媽媽資訊的ajaxSimulator:

```javascript
const checkDad=()=>{
    (省略)
}

useEffect(() => {
    /* 下面是 componentDidMount */
    ajaxSimulator();
    /* 上面是 componentDidMount */  
}, []);

if(isRightDad===false){
    (省略)
}
```
        
step 3 : 替代Baby.js中的componentDidMount+componentDidUpdate
因為我們在第一次設定爸爸跟更改爸爸時都要檢查爸爸是否正確，所以我們要用useEffect一起替代componentDidMount + componentDidUpdate。

現在加入一個useEffect替代componentDidMount + componentDidUpdate。

在這邊，我們讓Baby.js的props中的爸爸第一次被渲染以及dad被改變時，用checkDad去檢查爸爸正確性，以決定是否提供寶寶的媽媽資訊:

```javascript
const checkDad=()=>{
    (省略)
}

useEffect(() => {
    ajaxSimulator();    
}, []);

useEffect(() => {
    /* 下面是 componentDidMount和componentDidUpdate */
    
    checkDad();
    
    /* 上面是 componentDidMount和componentDidUpdate */    
}, [props.dad]); /* 加入監控的props.dad */ 

if(isRightDad===false){
    (省略)
}
```

step 4 : 替代Baby.js中的用來去除喊聲爸componentWillMount
現在先在剛剛專為ajax的useEffect加入喊聲爸的函式，讓我們等等可以用componentWillMount移除。
```javascript
useEffect(() => {
    ajaxSimulator();
    document.getElementById("talk").append('爸!')
}, []);
```
- 這樣按很多次之後就會呈現跟之前很像的一堆「爸!」狀況:


在useEffect中，第一個參數函式的return就是componentWillUnmount，
也就是第一個參數函式的return值也要是一個函式:
```javascript
useEffect(() => {
    ajaxSimulator();
    document.getElementById("talk").append('爸!')
    
    return(()=>{
        /* 下面是 componentWillUnmount */

        document.getElementById("talk").innerHTML="";
        
        /* 上面是 componentWillUnmount */
    })
}, []);
```


