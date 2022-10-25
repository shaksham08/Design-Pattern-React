# Design-Pattern-React
My Learning of design pattern in React JS

## 1. Singleton

- Means a single intance of a object 
- Lets say we are working on react app and there is a backend team working in backend.
- Our task is to connect or establish some socket connection with backend
- So we would make a class socket and this would make a connection with server.
- Here the problem is that react component have to rerender so there are chances that we would be making multiple connection to the backend.
- So a normal developer would take a approch like this 

- Below is from  Socket Js file 

```js
class SocketService {
  constructor() {
    console.log("IO CONNECTION BUILT");
  }

  sendData() {
    console.log("SEND DATA on IO");
  }
}

a
export default SocketService;
```

- Below is from App js file 

```js
import "./styles.css";
import SocketService from "./socket"
import React, {useState} from "react"

export default function App() {

  const [state,setState] = useState(1);

  const socket = new SocketService();
  return (
    <div className="App">
      <button onClick={()=>setState(data=>data +1)}>{state}</button>
      <h1>Hello CodeSandbox </h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
  );
}
```

- So socket connection will be made multiple times whenever our component re renders

- To solve this we can make this change 

```js
let instance = null

class SocketService {
  constructor() {
    if(!instance){
      console.log("IO CONNECTION BUILT");
      instance = this;
    }
    return instance;
  }

  sendData() {
    console.log("SEND DATA on IO");
  }
}


export default SocketService;
```

- So in singleton we force class or a service to be created only one instance, now we need not to worry about its multiple instance creation.