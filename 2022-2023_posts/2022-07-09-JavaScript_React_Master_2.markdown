---
title: "{ ReactJS } TypeScript with styled-component"
date: 2022-07-09 12:20:00 +07:00
tags: [TypeScript, ReactJS, Styled-components, Props, State, Forms, Themes]
description: "Styled Components + TypeScript + ReactJS."
---

**TypeScript with Styled-component**

<em> How to type functions and arguments?</em>

- 'Proptypes' allows you to check the types of props after codes run (JS).
- While, 'interface' allows you check the types of props before the codes run (TS is a strongly typed language).
- Interface: The explanation of the shapes of the props.

```javascript
// 사용 될 props 의 타입을 정의.
interface ContainerProps {
  bgColor: string;
}
```

<em> How to pass and receive props from components? </em>

- It is not that different from what we did in JS
- But, we can define 'CircleProps'

```javascript
    // Pass the props
    root.render(
    <div>
    <Circle bgColor="tomato" />
    <Circle bgColor="teal" />
    </div>
    );

    ...
    // Receive the props
    const Circle = styled.div<ContainerProps>`
    ...
    background-color: ${(props) => props.bgColor};
    `;
```

<em> How to make props required or optional? </em>

```javascript
// All Default props.
interface ContainerProps {
  bgColor: string;
  borderColor: string; // It should be there
}

// BorderColor prop is optional.
interface CircleProps {
  bgColor: string;
  borderColor?: string; // It might not be there.
  text?: string;
}

const Container =
  styled.div <
  ContainerProps >
  `
    width: 200px;
    height: 200px;
    background-color: ${(props) => props.bgColor};
    border-radius: 100px;
    <!-- Because this should take any value --!> 
    border: 3px solid ${(props) => props.borderColor}; 
`;

// You also can define the default value to props
function Circle({ bgColor, borderColor, text = "default text" }: CircleProps) {
  return (
    <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>
      {text}
    </Container>
  );
}

export default Circle;
```

**More Practice**

### Source Code (own practice)

```javascript
import styled from "styled-components";

interface PlayerProps {
  name: string;
  age: number;
}

interface HelloProps {
  txtColor: string;
}

const HelloText =
  styled.p <
  HelloProps >
  `
    color: ${(props) => props.txtColor};
`;

const sayHello = (playerObj: PlayerProps) =>
  `Hello ${playerObj.name}. you are ${playerObj.age} old. `;

function Hello({ txtColor }: HelloProps) {
  return (
    <HelloText txtColor={txtColor}>
      {sayHello({ name: "jaydon", age: 27 })}
      <br></br>
      {sayHello({ name: "Rachel", age: 25 })}
    </HelloText>
  );
}

export default Hello;
```

#### Forms (How to handle events?)

- SyntheticEvent
  - How the type system look like in the library (ReactJS)?
  ```javascript
  const onClick = (event: React.FormEvent<HTMLButtonElement>);
  ```
  - event: React = namespace
  - FormEvent = A type of event you will use
  - `<HTMLButtonElement>` = An element which is going to make the event happens
  - e.g. If a button is outside of form, the type of event will be MouseEvent.

### Source Code

```javascript
function App() {
  const [value, setValue] = useState("");
  // This means the function is going to be executed from changes of Input element.
  const onChange = (event: React.FormEvent<HTMLInputElement>) => {
    // Open up event and get the value of currentTarget
    const {
      currentTarget: { value },
    } = event; // 동시에 param 의 타입을 string 으로 명시해줌

    setValue(value);
  };

  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("hello " + value);
  };

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          value={value}
          onChange={onChange}
          type="text"
          placeholder="username"
        />
        <button>Log in</button>
      </form>
    </div>
  );
}

export default App;
```

#### Connect TypeScript with Theme

### Source Code

```javascript
// styled.d.ts - declarations
import 'styled-components';

// Explain your theme shape
declare module 'styled-components' {
    export interface DefaultTheme {
        textColor: string;
        bgColor: string;
        btnColor: string;
    }
}
```

```javascript
// theme.ts - theme css
import { DefaultTheme } from "styled-components";

export const lightTheme: DefaultTheme = {
  bgColor: "white",
  textColor: "black",
  btnColor: "tomato",
};

export const darkTheme: DefaultTheme = {
  bgColor: "black",
  textColor: "white",
  btnColor: "teal",
};
```

```javascript
// App.tsx
import styled from "styled-components";

// App 이 ThemeProvider 안에 있으니..
// theme.ts 에 있는 theme 의 모든 properties 에 접근 가능하다.
const Container = styled.div`
  background-color: ${(props) => props.theme.bgColor};
`;

const H1 = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

function App() {
  return (
    <Container>
      <H1>Text</H1>
    </Container>
  );
}
```

- <em>Theme prop 을 전달 함으로서 변경가능</em>

```javascript
root.render(
  <div>
    <ThemeProvider theme={darkTheme}>
      <App />
    </ThemeProvider>
  </div>
);
```
