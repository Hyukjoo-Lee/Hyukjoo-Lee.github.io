---
title: "{ ReactJS } Basic React 3"
date: 2022-06-03 18:00:00 +07:00
tags: [JavaScript, ReactJS, Props, Hooks]
---

**React Props & React Hooks**

## React Props
- Props are arguments passed into React components.
- To pass props into a component, use the same syntax as HTML attributes. e.g. <Button text= {"Send"}>
- React.js does not know the props' type = does not occur any error, even if you pass the wrong data type.
    - PropType: Allows you to check each different types.

#### Source Code
```html
<script>
    /*
    shortCut : 'props.text' => '{ text }'
    fontSize = 14 is the default value
    */
    function Btn({ text, fontSize = 14 }) {
        return <button
            style={{
                backgroundColor: "tomato",
                color: "white",
                padding: "10px 20px",
                border: 0,
                borderRadius: 10,
                fontSize: fontSize
            }}
        >{text}</button>
    }

    // We can set propType - is going to allow us to check which 'types' of data should be received as props
    // Also we can set isRequired which means 'Should have a value'; without a value will show an warning message
    Btn.propTypes = {
        text: PropTypes.string.isRequired,
        fontSize: PropTypes.number,
    };

    function App() {
        return (
            <div>
                <Btn text="Save Changes" fontSize={18} />
                <Btn fontSize={14} text={"Continue"} />
            </div>
        );
    }

    const root = document.querySelector(".root");
    ReactDOM.render(<App />, root);
    </script>
```

## CSS modules

- There were two ways;
    1. Create styles.css, then import "./styles.css"
    2. Using HTML syntax like..  ```html <button style={{ ... }} > ```
- Now, using CSS modules, we can create an isolated css file for a specific component.
- The CSS inside a module is available only for the component that imported it, and you do not have to worry about name conflicts.
- 예를들어 Button.module.css 에서도 클래스 이름 title 를 사용하고, App.module.css 에서도 클래스 이름 title 를 사용한다고 가정해 봤을 때, 같은 클래스 이름이라도 다르게 스타일을 적용할 수 있다.

#### Source Code
```javascript
import Button from "./Button";
// import "./styles.css"; 대신에 따로 css 파일을 만들어 적용 할 수 있다.
import styles from "./App.module.css";
import secondStyles from "./SecondApp.module.css";

function App() {
  return (
    <div>
      <h1 className={styles.title}>Title 1</h1>
      <h1 className={secondStyles.title}>Title 2</h1>
      <Button text={"Continue"} />
    </div>
  );
}

export default App;

```

## React Hooks
- Hooks allow function components to have access to state and other React features. 
- useState = Hook to keep track of the application state.

## useEffect

- Normally, everytime when a state changes, the entire component is rendered again = Codes runs repeatedly
- How we can render a component at ONCE when a condition is changed ?
- For example, when we get the data from the API, it is better to get API only once.
- There are two arguments
    - useEffect(1ST, [ 2ND ])
        - 1st Argument: A function which allows you running 'ONE' specific function at once a time.
        - 2nd Argument: dependencies = things that React.js should watch. They can be one OR more states that tells React.js like. if a specific 'keyword' changes, run a function in the 1st argument.

#### Source Code

```javascript
import { useState, useEffect } from "react";

/**
 * state 가 변화 할 때마다 전체 App Component는 re-render 된다.
 * 앱이 실행되었을 때, 처음에만 render 되게 어떻게 만들까?
 * 예를들어, API 를 통해 데이터를 가져올 때, API 를 한번만 가져오고 state가
 * 업데이트 될 때 마다 Component 가 계속 re-render 되는 걸 원치 않을 것
 * 이런경우에 useEffect 를 활용한다.
 */
function App() {
  const [counter, setValue] = useState(0);
  const [keyword, setKeyword] = useState("");

  const onClick = () => setValue(prev => prev + 1);
  const onChange = (event) => setKeyword(event.target.value);

  useEffect(() => {
    console.log("I run only once");
  }, []);

  useEffect(() => {
    console.log("I run when 'keyword' changes");
  }, [keyword]);

  useEffect(() => {
    console.log("I run when 'counter' changes");
  }, [counter]);

  return (
    <div>
      <input
        value={keyword}
        onChange={onChange}
        type="text"
        placeholder="Search here..."
      />
      <h1>{counter}</h1>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}

export default App;
```

### useEffect cleanup function
  - It is not used commonly, but it is good to know.
  - A function which runs when a component is destroyed.
  - Basically, We return a function with an useEffect function. 

#### Source Code

```javascript
import { useState, useEffect } from "react";

/**
 * Cleanup function = 어떠한 component 가 destroyed 될 때 실행되는 함수
 */
function Hello() {

  // function destroyedFn() {
  //   console.log("bye :( ")
  // };

  // function effectFn() {
  //   console.log("I am Here");
  //   return destroyedFn;
  // };

  // useEffect(effectFn, []);

  // Make codes above shorter
  useEffect(() => {
    console.log("hi :)");
    return () => console.log("bye :)");
  })

  return <h1>Hello</h1>;
}

function App() {

  const [showing, setShowing] = useState(false);
  const onClick = () => setShowing((prev) => !prev);

  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;
```