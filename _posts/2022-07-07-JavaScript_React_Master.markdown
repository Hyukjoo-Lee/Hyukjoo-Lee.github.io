---
title: "{ ReactJS } React styled-components"
date: 2022-07-07 12:20:00 +07:00
tags: [JavaScript, ReactJS, Styled-components]
description: "About Styled components."
---

**React styled-components**

### What I learned

1. What is styled-component?
2. How to use it.
3. How to create 'configurable property' using props.
4. How to add attributes.
5. How to bring all of properties from a component.
6. How to change tag (e.g. div => header); But same style applied

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
