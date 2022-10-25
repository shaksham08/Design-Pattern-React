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


## 2. Proxy Design pattern

- here we are updating id of someting on state change which is not good 

```js
import "./styles.css";
import SocketService from "./socket"
import React, {useState} from "react"

const person = {
  name : "shaksham", 
  age : 23,
  balance : 100 , 
  id : 1
}

export default function App() {

  const [state,setState] = useState(1);

  const socket = new SocketService();
  return (
    <div className="App">
      <h1>{person.name}</h1>
      <button onClick={()=>setState(data=>data +1)}>{state}</button>
      <h1>Hello CodeSandbox </h1>
      <h2>{person.id}</h2>
      <button onClick={()=>{
        person.id =2;
        setState(data=>data +1);
      }}> change</button>
    </div>
  );
}

```


- Here we can create a proxy pattern to stop user from changing id but user can change the name and other details 
- Here we create a middle ware and block the direct access to the object from the user.
- All the crud operation should happen though this proxy.


```js
import React from "react";
import logo from "./logo.svg";
import "./App.css";

let isAdmin = TextTrackCue;

const person = {
  id: 1,
  name: "Shaksham",
  age: 23,
  balance: 100,
};

const personProxy = new Proxy(person, {
  set: (obj, prop, value) => {
    if (prop == "name") {
      obj[prop] = value;
    } else {
      throw new Error("You can only change name property");
    }
  },
  get: (obj, prop) => {
    if (prop == "balance") {
      if (isAdmin) return obj[prop];
      return null;
    }
    return obj[prop];
  },
});

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>{personProxy.id}</h1>
        <h1>{personProxy.name}</h1>
        <h1>{personProxy.balance}</h1>
        <button
          onClick={(e) => {
            personProxy.id = 2;
            setState((s) => s + 1);
          }}
        >
          Change
        </button>
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.jsx</code> and save to reload!
        </p>
        <span className="App-link">Hello from codedamn :)</span>
      </header>
    </div>
  );
}

export default App;

```