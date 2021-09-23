###### tags: `React`
# Attributes of tag input
[TOC]

If we have a function Component `logingForm.js`.
```jsx
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

## `defaultvalue`

```jsx
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

## `value` and state member data 

What is difference btw `value` and `defaultValue` ? 
> value is dependent on static member data

```jsx
return (
    <div>
        <input type="text" defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>
        <div>
            目前account:{account}
        </div>
        // button to set account value to ""
        <button onClick={()=>{setAccount("")}}>用按鍵取得新的account</button>
    </div>
)
```
- `defaultValue`只是初始值，元件被建立後它就不會影響輸入值。
- `onChange`只有在使用者改變input時才會觸發，和它用來改變的state無關

![](https://i.imgur.com/vsXj7hh.gif)

如果你希望「input中的值只在一開始受`state`影響」，就要用該`state`去綁定`defaultValue`；相反的，如果你希望「input中的值始終跟著`state`」，就要用該`state`去綁定`value`。

input中的值始終跟著state，state的值也隨input值改變而更動 : 這樣的狀況我們會稱為控制組件/受控組件, 如下
```jsx
import React,{useState} from "react";
const LoginForm=()=>{
    const [account,setAccount]=useState("快來輸入我");

    return (
        <div>
            // value will keep listening what user type
            <input type="text" value={account} onChange={(e)=>{setAccount(e.target.value)}}/>
            <div>
                目前account:{account}
            </div>
            // set `value` to ""
            <button onClick={()=>{setAccount("")}}>用按鍵取得新的account</button>
        </div>
    )
}
export default LoginForm;
```

![](https://i.imgur.com/GuaagzK.gif)


## disabled

語法
```jsx
<input type="text" disabled={true} defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>

<input type="text" disabled defaultValue={account} onChange={(e)=>{setAccount(e.target.value)}}/>
```

![](https://i.imgur.com/KR7vnAe.png)   
- With `disabled` we can't type/input any character/symbol/integer into the input bar   



`disable`和`componentDidMount`搭配時容易發生的錯誤使用    
如果你想要在`componentDidMount`中去取得初始input值(一般發生在用`fetch`去取得該資料),那麼你不該使用`defaultValue`來設定。以下是用`componentDidMount`+`defaultValue`的狀況:   
```jsx
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

Flow
在render return前決定state初始值 -> 在render return時決定`defaultValue`值 -> 畫面彩現 -> 執行`componentDidMount`，在其中改變state值 -> `defaultValue`值不變
- 因為在`componentDidMount`中設定state等同於我們在非input處修改state的狀況，所以如果你要讓input值等同從server取得的值，應該要用value來綁定。

如果綁了state在value上而沒有綁`onChange`?
在這個狀況下，input會鎖死變成無法修改的狀態。你只能透過在從其他地方更改該state來修改input中的值。


## textarea

- `<textarea> ... </textarea>`和`<input type="text"/>`的用法是一模一樣的。

```jsx
<textarea value={account} onChange={(e)=>{setAccount(e.target.value)}}></textarea>
```

## `<select>` and `<option>`

`select`和`option`是要在`select`中設定`value`、`onChange`、`defaultValue`。

特別的地方是當`value`、`defaultValue`的值被指定為不是存在任一`option`中的值時，就不會顯示該值，而是顯示第一個`option`的值
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

另外，你也可以在`option`透過`selected`這個props來控制預選取的`option`，但是當`<select>`有設定value或defaultValue時，以`<select>`的設定值為主。
```jsx
<select onChange={(e)=>{setNowSelect(e.target.value)}}>
    <option value="123">123</option>
    <option selected={true} value="456" >456</option>
</select>

// or 
<option selected value="456" >456</option>
```

## `checked`

這兩個比較特別，它們是用`checked`這個`props`去控制是否被選取。

語法
```jsx
// boolean value inside
checked={boolean} 

// expression insdie
checked={expression_true_or_false}

// for example
// set defaultv boolean value is false
const [isCheck,setIsCheck]=useState(false);
<input type="radio" value="123" checked={isCheck} onChange={(e)=>{setIsCheck(true)}}   /> 123 <br/>
<input type="radio" value="456" checked={!isCheck} onChange={(e)=>{setIsCheck(false)}} /> 456
```

因為`checked`是`boolean`去控制，我們如果要取得value，用「比較是否和value相同」的方式來設定會比較方便，就不需要多用一個`state`去存目前的value。
```jsx
const [nowSelect,setNowSelect]=useState("789");
<input type="radio" value="123" checked={nowSelect==="123"} onChange={(e)=>{setNowSelect("123")}} /> 123 <br/>
<input type="radio" value="456" checked={nowSelect==="456"} onChange={(e)=>{setNowSelect("456")}} /> 456 
```

## `onSubmit` for `<form>`
form的subimt要觸發的事件是用`onSubmit`去綁定函式
```jsx
<form onSubmit={this.handleSubmit}>
  <input type="submit" value="Submit" />
</form>
```
