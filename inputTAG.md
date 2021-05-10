###### tags: `React`
# Attributes in `<input\>
[TOC]



If we have a function Component logingForm.js.
```javascript
import React,{useState} from "react";
const LoginForm=()=>{
    // initial state member 'account'
    //    to save input value
    const [account,setAccount]=useState("");
    return (
        <div>
            // e.target.value saves input value
            <input type="text" onChange={(e)=>{setAccount(e.target.value)}}/>
            <div>
            //show up the account 
                目前account:{account}
            </div>
        </div>
    )
}
export default LoginForm;
```

## default value

```javascript=
import React,{useState} from "react";
const LoginForm=()=>{
    const [account,setAccount]=useState("快來輸入我");
    const [password,setPassword]=useState("");
    
    return (
        <div>
            <input type="text" defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>
            <div>
                目前account:{account}
            </div>
        </div>
    )
}
export default LoginForm;
```
![](https://i.imgur.com/715vwzI.png)

## value and state member data 

What is difference btw `value` and `defaultValue` ? 
> value is dependent on static member data

```javascript
return (
    <div>
        <input type="text" defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>
        <div>
            目前account:{account}
        </div>
        // it will clear defaultValue
        <button onClick={()=>{setAccount("")}}>用按鍵取得新的account</button>
    </div>
)
```
- 因為defaultValue只是初始值，元件被建立後它就不會影響輸入值。
    - 而onChange只有在使用者改變input時才會觸發，和它用來改變的state無關
![](https://i.imgur.com/vsXj7hh.gif)


如果你希望「input中的值只在一開始受state影響」，就要用該state去綁定defaultValue；相反的，如果你希望「input中的值始終跟著state」，**就要用該state去綁定value。「input中的值始終跟著state，state的值也隨input值改變而更動」這樣的狀況我們會稱為控制組件(or 受控組件):**
```javascript
import React,{useState} from "react";
const LoginForm=()=>{
    const [account,setAccount]=useState("快來輸入我");

    return (
        <div>
            <input type="text" value={account} onChange={(e)=>{setAccount(e.target.value)}}/>
            <div>
                目前account:{account}
            </div>
            <button onClick={()=>{setAccount("")}}>用按鍵取得新的account</button>
        </div>
    )
}
export default LoginForm;
```

![](https://i.imgur.com/GuaagzK.gif)
- after invoking `onClick`, the `<input\>` bar will show as `value` attribute showed which is dependent on state member account 

## disabled

just simply add `disabled` attribute in `<input/>` tag
```javascript=
<input type="text" disabled={true} defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>
// or 
<input type="text" disabled defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>
```

Now we can't type/input any character/symbol/integer into the bar
![](https://i.imgur.com/KR7vnAe.png)


和componentDidMount搭配時容易發生的錯誤使用

- 如果你想要在componentDidMount中去取得初始input值(一般發生在用fetch去取得該資料)，
    > 那麼你不該使用defaultValue來設定。以下是用componentDidMount+defaultValue的狀況:
```javascript
import React,{useState,useEffect} from "react";
const LoginForm=()=>{
    const [account,setAccount]=useState("快來輸入我");
    
    useEffect(()=>{
        setTimeout(()=>{setAccount("用fetch拿到的資料")},2000);
    },[])

    return (
        <div>
            <input type="text" defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>
            <div>
                目前account:{account}
            </div>
            <button onClick={()=>{setAccount("")}}>用按鍵取得新的account</button>
        </div>
    )
}
export default LoginForm;
```
![](https://i.imgur.com/c0Ga6wx.gif)
在render return前決定state初始值 -> 在render return時決定defaultValue值 -> 渲染畫面 -> 執行componentDidMount，在其中改變state值 -> defaultValue值不變
- 因為在componentDidMount中設定state等同於我們在非input處修改state的狀況，所以如果你要讓input值等同從server取得的值，應該要用value來綁定。

如果綁了state在value上而沒有綁onChange?
在這個狀況下，input會鎖死變成無法修改的狀態。你只能透過在從其他地方更改該state來修改input中的值。


## textarea
`<textarea></textarea>`和`<input type="text"/>`的用法是一模一樣的。

```jsx
<textarea value={account} onChange={(e)=>{setAccount(e.target.value)}}></textarea>
```

## select
select和option是要在select中設定value、onChange、defaultValue。特別的地方是當value、defaultValue的值被指定為不是存在任一option中的值時，就不會顯示該值，而是顯示第一個option的值

![](https://i.imgur.com/ouUrIaV.gif)

```javascript
import React,{useState} from "react";
const LoginForm=()=>{
    const [nowSelect,setNowSelect]=useState("789");

    return (
        <div>
            <select value={nowSelect} onChange={(e)=>{setNowSelect(e.target.value)}}>
                <option value="123">123</option>
                <option value="456">456</option>
            </select>
            <div>
                目前select:{nowSelect}
            </div>
            <button onClick={(e)=>{setNowSelect("789")}}>改變為789</button>
        </div>
    )
}
export default LoginForm;
```

另外，你也可以在option透過selected這個props來控制預選取的option，但是當select標籤有設定value或defaultValue時，以select標籤的設定值為主。
```jsx
<select onChange={(e)=>{setNowSelect(e.target.value)}}>
    <option value="123">123</option>
    <option selected={true} value="456" >456</option>
</select>
// or 
<option selected value="456" >456</option>
```

## checked
這兩個比較特別，它們是用checked這個props去控制是否被選取。
```javascript=
checked={boolean} 
// or 
checked={expression_true_or_false}
```

```javascript
const [isCheck,setIsCheck]=useState(false);
<input type="radio" value="123" checked={isCheck} onChange={(e)=>{setIsCheck(true)}} />123<br/>
<input type="radio" value="456" checked={!isCheck} onChange={(e)=>{setIsCheck(false)}} />456
```
因為checked是用布林值去控制，我們如果要取得value值，用「比較是否和value相同」的方式來設定會比較方便，就不需要多用一個state去存目前的value。

```jsx
const [nowSelect,setNowSelect]=useState("789");
<input type="radio" value="123" checked={nowSelect==="123"} onChange={(e)=>{setNowSelect("123")}} />123<br/>
<input type="radio" value="456" checked={nowSelect==="456"} onChange={(e)=>{setNowSelect("456")}}/>456
```
## form to submit
form的subimt要觸發的事件是用`onSubmit`去綁定函式
```javascript=
<form onSubmit={this.handleSubmit}>
  <input type="submit" value="Submit" />
</form>
```