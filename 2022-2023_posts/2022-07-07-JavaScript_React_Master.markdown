---
title: "{ ReactJS } React styled-components"
date: 2022-07-07 12:20:00 +07:00
tags: [JavaScript, ReactJS, Styled-components]
description: "About Styled components."
---

**React styled-components**

### Introduction of Styled-component

<em>What is styled-component?</em>

- Styled-components is a popular library that is used to style React applications
- It allows you write actual CSS in your JavaScript.

<em>How to use styled-components in a React application.</em>

- 1st. Import styled-component.
- 2nd. Name of the HTML tag with dot(.).
- 3rd. Write normal css code inside backtick(``).

<em>How to create 'configurable property' using props.</em>

- background-color: ${(props) => props.bgColor}; 2)`<Box bgColor="red" />`

<em>How to add common attributes.</em>

- We know the same attributes are repeated usually.
- e.g. Many Input with <em>'required:true'</em>
- Instead of writing required many times, just add once like below.
- const Input = styled.input.attrs({ required: true})

<em>How to extend a component (bringing all of properties from a component.)</em>

- `` const Circle = styled(Box)`border-radius: 50px`  ``

<em>How to change only HTML tag (e.g. div => header); But same style applied (스타일을 그대로 가져오면서 html 태그명만 변경하는법)</em>

- using 'as' = props
- e.g. `<Button as="a">`

### Source Code

```javascript
// Styled components: styled-components allows you write actual CSS in your JavaScript.
import styled from "styled-components";

// 1. Import styled-components => HTML Tags => Backtick (``) => CSS code inside.
const Father = styled.div`
  display: flex;
`;

const Btn = styled.button`
  color: white;
  background-color: tomato;
  border: 0;
  border-radius: 15px;
`;

const Text = styled.span`
  color: white;
  font-size: 30px;
`;

// 2. Create 'configurable property' using props.
const Box = styled.div`
  background-color: ${(props) => props.bgColor};
  width: 200px;
  height: 200px;
  text-align: center;
`;

// 3. Add attributes
const Input = styled.input.attrs({ required: true, value: "Jaydon" })`
  background-color: tomato;
`;

// 4. Bring all of properties from Box
const Circle = styled(Box)`
  border-radius: 50px;
`;

function App() {
  return (
    // 5. Change tag (div => header); But same style applied
    <Father as="header">
      {/* <Btn>Log in</Btn> */}

      {/* Change tag (Button => Link) */}
      <Btn as="a" href="/">
        Log in
      </Btn>

      {/* Pass props value */}
      <Box bgColor="teal" />
      <Circle bgColor="tomato" />

      <Input />
    </Father>
  );
}

export default App;
```

#### Animation: Same as CSS

- Animation 을 받을 element 를 만들어 주고, 전달 될 animation 을 만들어 주면 된다.
- Using keyframes helper
  `` const rotateAnimation = keyframes`from{}to{}`  ``
  `` const Box = styled.div`animation: ${rotateAnimation} 5s linear infinite`  ``

#### Pseudo selector

- 예를들어 Wrapper 가 부모인 h1 tag 를 선택해서 디자인 할때,

```javascript
const Wrapper = styled.div`

h1 {
  color: red;
}

&:hover {
  font-size: 100px;
}
<!-- OR -->
${Title}:hover {
font-size: 100px;
}
`;
와 같은 구조가 가능함.
```

### Source Code

```javascript
import styled, { keyframes } from "styled-components";

const Wrapper = styled.div`
  display: flex;
  height: 100vh;
  width: 100vm;
  justify-content: center;
  align-items: center;
`;

// 전달 될 animation 1
const rotateAnimation = keyframes`
0% {
  transform: rotate(0deg);
  border-radius: 0px;
}
50% {
  transform: rotate(360deg);
  border-radius: 200px;
}
100% {
  transform: rotate(0deg);
  border-radius: 0px;
}
`;

// 전달 될 animation 2
const changeColorAnimation = keyframes`
from {
  color: darkblue;
} to {
  color: red;
}
`;

const Emoji = styled.span`
  font-size: 22px;
`;

// 전달 받을 Element
const Box = styled.div`
  height: 400px;
  width: 400px;
  background-color: ${(props) => props.bgColor};
  display: flex;
  justify-content: center;
  align-items: center;

  animation: ${rotateAnimation} 5s linear infinite, ${changeColorAnimation} 1s
      infinite;

  /* shortcut */
  ${Emoji}:hover {
    font-size: 40px;
  }
`;

const Circle = styled(Box)`
  border-radius: 50px;
  font-size: 50px;
  ${Emoji}:hover {
    font-size: 100px;
  }
`;

function App() {
  return (
    <Wrapper as="header">
      <Box bgColor="tomato" rotate="rotateAnimation">
        <Emoji>😻LoveYa😻</Emoji>
      </Box>
      <Circle bgColor="teal">
        Rachel
        <Emoji>💘</Emoji>
      </Circle>
    </Wrapper>
  );
}

export default App;
```

#### Theme

- Dark Mode (50%) & Local Estate Management (50%)

**Dark Mode**

```javascript
// 1. Import
import { ThemeProvider } from "styled-components";

// 2. Create Object with properties
const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
};

root.render(
  <React.StrictMode>
    {/* 3. Send the object to ThemeProvider;
    Every components inside ThemeProvider is going to access object */}
    <ThemeProvider theme={lightTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>
);

// 4. Inside App.js
const Wrapper = styled.div`
  ... 
  // Receive props; name should be matched 
  background-color: ${(props) => props.theme.backgroundColor};
`;
```
