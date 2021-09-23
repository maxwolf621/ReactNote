###### tags: `React`
# Function Component

Component in React 分為 function component 以及  class component


For example :: a function component
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import * as serviceWorker from './serviceWorker';

/* 加入下面這一段 */
function App(){
 return(
   <button>Me_is_A_function</button>
 );
}
/* 加入上面這一段 */

/* 修改div中的東西，改為<App/> */
ReactDOM.render(
 <div>
     <App/>
 </div>,
 document.getElementById('root')
);
```

To use a component in the html we might ...
```jsx
<Component_name/>

// OR

<Component_name></Component_name>
```
- 元素名稱第一個字母必須要是大寫、和函式(或class)名稱相同，JSX才會知道這是自製的component

在這種方式下，我們就可以自己做元素，然後給別人or在其他地方重複使用。像下面的程式碼，就是自己做一個進度條:
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import * as serviceWorker from './serviceWorker';

function Progress(){
    const barWidth="50%";
    return(
        <div>
        <div className="progress-back" style={{backgroundColor:"rgba(0,0,0,0.2)",width:"200px",height:"7px",borderRadius:"10px"}}>
          <div className="progress-bar" style={{backgroundColor:"#fe5196",width:barWidth,height:"100%",borderRadius:"10px"}}></div>
        </div>
      </div>
    );
}
/* 加入上面這一段 */

ReactDOM.render(
    <div>
        <Progress/>
    </div>,
    document.getElementById('root')
);

serviceWorker.unregister();
```

- 之後如果要用這個進度條，就不用再重複寫一次，用`<Progress/>`就好。


## The good of Component

we can use every single component as a `.js` file

for example
```bash=
index.html
|____ index.js

# ** With Component 

index.html
|____ index.js
        |____ 元件.js
```
實際上的做法如下:

以module的方式分檔，藉由export default App;把App函式輸出給引入的對象。注意因為它是React component，還是要以import React from 'react';引入React。
請打開src資料夾底下的App.js，並修改為:
```jsx
import React from 'react';

function App(){
  return(
    <button>Button</button>
  );
}
export default App; //輸出App函式
```

import component into index.js
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App'; //引入App
import * as serviceWorker from './serviceWorker';


ReactDOM.render(
    <div>
        <App/>
    </div>,
    document.getElementById('root')
);
```
