---
title: "{ ReactJS } React Basic"
date: 2024-10-09 03:00:00 KST
tags: [React]
---

# 리액트 기초 정리 자료 - 팀 스터디

---

### 1. CSS 모듈 사용 예제

```jsx
import styles from "./Box.module.css";

export default function Box() {
  return (
    <>
      <div className={styles.box}></div>
    </>
  );
}
```

#### CSS 파일 (Box.module.css)

```css
.box {
  width: 200px;
  height: 50px;
  background-color: blue;
}
```

- `Box.module.css`에서 스타일을 가져와서 React 컴포넌트에 적용하는 예시입니다.
- `className`에 CSS 모듈을 적용하여 `box` 클래스를 사용할 수 있습니다.

---

### 2. `useState` 훅 사용 예제

```jsx
import { useState } from "react";

export default function Hello_2(props) {
  const [name, setName] = useState("Mike");
  const [age, setAge] = useState(props.age);
  const msg = age > 19 ? "성인입니다" : "미성년자입니다";

  console.log(props);

  return (
    <>
      <h2 id="age">{age}</h2>

      <button
        onClick={() => {
          setAge(age + 1);
        }}
      >
        나이 증가
      </button>

      <h1>state</h1>
      <h2 id="name">
        {name} {props.name}
      </h2>

      <button
        onClick={() => {
          setName(name === "Mike" ? "Jane" : "Mike");
        }}
      >
        이름 변경
      </button>
    </>
  );
}
```

- `useState` 훅을 사용하여 `name`과 `age`라는 상태(state)를 정의하고, 버튼 클릭 시 상태가 변경되도록 하고 있습니다.

---

### 3. 부모 컴포넌트에서 `age` 전달하기

```jsx
import { useState } from "react";
import UserName from "./UserName";

export default function Hello({ age }) {
  const [name, setName] = useState("Mike");
  const [age_2, setAge_2] = useState(age);

  const msg = age > 19 ? "성인입니다." : "미성년자입니다";

  return (
    <>
      <h2>
        {name} ({age_2})
      </h2>

      <button
        onClick={() => {
          setAge_2(age_2 + 1);
        }}
      >
        나이 증가
      </button>

      <h2 id="name">
        {name} ({age}) : {msg}
      </h2>

      <UserName name={name} />
      <button
        onClick={() => {
          setName(name === "Mike" ? "Jane" : "Mike");
        }}
      >
        이름 변경
      </button>
    </>
  );
}
```

- 부모 컴포넌트에서 `age` 값을 전달받고, 상태 관리 및 버튼을 통해 상태를 업데이트하는 방법을 보여줍니다.

---

### 4. `map()` 함수로 리스트 렌더링

```jsx
import Movie from "./Movie";

const Map = () => {
  const movies = [
    { title: "title1", year: 2001 },
    { title: "title2", year: 2002 },
    { title: "title3", year: 2003 },
    { title: "title4", year: 2004 },
  ];

  const renderMovies = movies.map((movie) => {
    return <Movie movie={movie} />;
  });

  return (
    <>
      <h1>Movie List</h1>
      {renderMovies}
    </>
  );
};

export default Map;
```

#### `Movie` 컴포넌트

```jsx
const Movie = (props) => {
  return (
    <div className="movie" key={props.movie.title}>
      <div className="movie-title">{props.movie.title}</div>
      <div className="movie-year">{props.movie.year}</div>
    </div>
  );
};

export default Movie;
```

- `map()` 함수를 사용하여 영화 리스트를 렌더링하는 방법을 설명합니다.

---

### 5. 간단한 컴포넌트 렌더링 예시

```jsx
export default function UserName({ name }) {
  return <p>Hello, {name}</p>;
}
```

- `UserName` 컴포넌트는 `name`을 받아서 간단한 텍스트를 렌더링하는 예시입니다.

---

### 6. 스타일링 예시

#### `App.css`

```css
.movie {
  display: flex;
  align-items: center;
  border: 1px solid gray;
  border-radius: 5px;
  padding: 5px;
  margin-bottom: 5px;
}

.movie .movie-title {
  flex-grow: 1;
  font-size: 0.5rem;
  color: red;
}
```

- 영화 리스트의 스타일을 지정한 예시입니다. `display: flex`를 사용하여 정렬하고, 타이틀에만 색과 크기를 다르게 적용했습니다.

---
