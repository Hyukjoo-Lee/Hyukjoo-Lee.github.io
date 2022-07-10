---
title: "{ ReactJS } TypeScript with styled-component"
date: 2022-07-09 12:20:00 +07:00
tags: [TypeScript, ReactJS, Styled-components, Props, State, Forms, Themes] 
description: "Styled Components + TypeScript + ReactJS."
---

**TypeScript with Styled-component**

```javascript
import styled from "styled-components";

interface ContainerProps {
    bgColor: string;    
}

const Container = styled.div<ContainerProps>`
    width: 200px;
    height: 200px;
    background-color: ${(props) => props.bgColor};
    border-radius: 100px;
`;

// Interface : The explanation about the object shape of props
// 사용 될 props 의 타입을 정의.
interface CircleProps {
    bgColor: string;
}

function Circle({bgColor}:CircleProps) {
    return <Container bgColor={bgColor} />
}

/**
 * PropTypes VS Interface
 * 'proptypes' show an error message after codes run
 * while, 'interface' shows an error message before the codes run
 */
interface PlayerProps {
    name: string;
    age: number;
}

const sayHello = (playerObj: PlayerProps) =>
    `Hello ${playerObj.name}. you are ${playerObj.age} old.`

sayHello({ name: "jaydon", age: 27 });
sayHello({ name: "rachel", age: 26 });

export default Circle;
```

### Source Code

### Source Code  (own practice)
```javascript
import styled from "styled-components";

interface PlayerProps {
    name: string;
    age: number;
}

interface HelloProps {
    txtColor: string;
}

const HelloText = styled.p<HelloProps>`
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

### Source Code (index.tsx)
```javascript 
root.render(
  <div>
    <Circle bgColor="tomato" />
    <Circle bgColor="teal" />
    <Hello txtColor='blue' />
    <Hello txtColor='red' />
  </div>
);

```