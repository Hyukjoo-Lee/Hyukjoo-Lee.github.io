---
title: "{ ReactJS } Basic React"
date: 2022-05-31 20:30:00 +07:00
tags: [JavaScript, ReactJS, Elements, Installation]
---

**React.js**

### What is React?

- React is a javascript library for building user interface
- By breaking it up into small reusable pieces of code called 'Components'
- Componenets for the pieces of the UI that you will likely reuse over and over again
- The other big benefit is that React keeps your application's data or at state
- and the UI in-sync and can efficiently update your UI when data changes.

### Features

**VanillaJS vs ReactJS**
- React does not create actual DOM nodes
    - Vanilla JS: HTML에 존재하는 elements 들을 <em>직접</em> 찾아서 업데이트

#### example code

```html
<body>
    <span>Total clicks: 0</span>
    <button id="btn"> Click me </button>
</body>
<script>
    let counter = 0;

    const button = document.getElementById("btn");
    const span = document.querySelector("span");
    function handleClick() {
        counter = counter + 1;
        span.innerText = `Total clicks: ${counter}`;
    }
    button.addEventListener("click", handleClick);

</script>
```

- React JS: 직접 페이지에 보여질 elements 들을 create 하고 rendering

#### example code
```html
<body>
    <div id="root"></div>
</body>
...
<script>
    const root = document.getElementById("root");
    const span = React.createElement(
     "span",
     { id: "a-span",
     style: {color: "red" } },
     "Hello I am a span"
    );
    ReactDOM.render(span, root); // show it to the user (render)
</script>

```

### What you must know about React


- Components, applications of states, organizing and managing your UI code and other core features of React
- Core features of React: useState, useEffect, useCallback, props, router...etc

### How to install and get set up with React (On Windows)
- With a tool called Create React App (npx create-react-app my-app); it automatically sets up a build system, and creates the files and folders you need to get started with React.

  ```powershell
    mkdir projectFolder
    cd projectFolder
    npx create-react-app myApp
    npm start
  ```

### Create a React Element

- React is only a library for creating and updating HTML elements in your UI.
- React provides the createElement function to create and return elements
- createElement has THREE arguments
    1) Type of element; usually a string which represents an HTML element or a tag (e.g. h1 element)
    2) Object containing any attributes and value you want to give the element; can pass an empty object, or the value null.
    3) Contents or children of the element
   ```javascript
    const span = React.createElement(
     "span", // 1
     { id: "a-span", // 2
     style: {color: "red" } },
     "Hello I am a span" // 3
    );
   ```
- React does not create actual DOM; it is an object representation of a DOM node
- It has several properties like key, props, and ref
  - props는 property의 약자로, 부모에게 받아온 데이터를 뜻 함

### Render an Element

- React uses a faster, easier, and friendlier way to create elements with a special syntax called JSX
- Mark-up-like syntax to create React elements
- We need the Babel compiler to transpile React elements with JSX.


