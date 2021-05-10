###### tags: `React`
# Http Request via fetch method

[More usage of fetch](https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API)
- Fetch is the Promise


```javascript
fetch( "Request_url", { 
        /* response's HTTP details */
        method : // "PUT" , "GET" , ...
        headers : //  ...
        // other contents 
    })
    // turn request to json
    .then(res => res.json()) 
    .then(data => {
          // Handle the request
    })
    
    .catch(e => {
        // Error Exception
    })
```


##### GET, POST ... method

To fetch a GET request
```javascript 
fetch( "request_url", 
        {method: "GET"}) 
    .then(res => res.json()) 
    .then(data => {
          //...
    })
    .catch(e => {
        //..
    }
```


#### headers objects

using `new Header` to create content in header

```javascript
fetch( request的url,
     {  method: "GET",
        headers: new Headers({
            'Content-Type': 'application/json',
        })
     })
    .then(res => res.json())
    .then(data => {
         //...
    })
    .catch(e => {
        //..
    })
    
```

#### Add A token in headers

```javascript=
// a jwt 
//    mark with Bearer
const token = "Bearer "+ TOKEN_FROM_CLIENT ;
```

```javascript
fetch( request的url, {
        method: "GET",
        headers: new Headers({
            'Content-Type': 'application/json',
            'Authorization': token, 
        })
    })
    .then(res => res.json())
    .then(data => {
          //...
    })
    .catch(e => {
        //...
    })
```


另外，token也能透過url以參數方式傳送。方式是url= 原url + '?token=' + 存好的token;，但一般還是會希望透過header傳token。


#### body Json type

用fetch在body傳送 json type的資料時，要把原資料字串化，接收者才會接到json格式的資料。

```
const data= { A:"資料A", B:"資料B" }
```
fetch( request的url, {
        method: "GET",
        body: JSON.stringify(data),   /*把json資料字串化*/
        headers: new Headers({
            'Content-Type': 'application/json'
        })
    })
    .then(res => res.json())
    .then(data => {
          /*接到request data後要做的事情*/
    })
    .catch(e => {
        /*發生錯誤時要做的事情*/
    })
    
    
#### x-www-form-urlencoded


```javascript
const data= { A:"資料A", B:"資料B" };
const formData = Object.keys(data).map(
    function (keyName) {
        return encodeURIComponent(keyName) + '=' + encodeURIComponent(data[keyName])
    }
).join('&');


fetch( request的url, {
        method: "GET",
        body: formData,   
        headers: new Headers({
            "Content-type": "application/x-www-form-urlencoded"
        })
    })
    .then(res => res.json())
    .then(data => {
         //...
    })
    .catch(e => {
        //..
    })
```



## Example

```javascript=
import React, { Component } from 'react';
class App extends Component {
  constructor(props) {
    super(props);
    this.state={
      repoName: null
    }
    this.handleClick=this.handleClick.bind(this);
  }

  
  handleClick(){
    fetch( 'https://api.github.com/users/jserv/repos',{method:"GET"})
    .then(res => res.json())
    .then(data => {
          // update our state
          //    which menas get the request's first element
          this.setState({repoName: data[0]['name']});
    })
    .catch(e => {
        console.log(e);
    })
  }
  
  render() {
      return (
        <div className="App">
          <div className="data-display">
            {(this.state.repoName===null)?"No Data":this.state.repoName}
          </div>
          <button onClick={this.handleClick}>取得jserv以英文字母排序的第一個repo</button>
    	</div>
    )
  }
};
```