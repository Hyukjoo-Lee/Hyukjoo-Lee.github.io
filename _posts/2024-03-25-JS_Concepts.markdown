---
title: "{ RN } JS concepts review"
date: 2024-03-25 21:00:00 +07:00
tags: [Async, Sync, JS]
# description: Notes of Transaction isolation level
---

## 자바스크립트 복습

- var - 함수 범위 또는 전역 범위에서 유효
- let - 해당 블록 내에서만 접근 가능
- const - 상수
- Object

```js
const person = {
  name: "Nata",
  age: 26,
  greet: () => {
    console.log("Hi " + this.name);
  },
  greet_2: function () {
    console.log("Hi " + this.name);
  },
};
// greet 메소드에서 this.name은 person 객체를 참조하는 것이 아니라 외부 스코프의 this를 참조
// arrow function는 상위 스코프의 this를 상속
person.greet(); // "Hi undefined"
// 전통적인 함수로 정의되었으므로 해당 메소드를 호출한 객체인 person 객체를 참조
person.greet_2(); // "Hi Nata"
```

### Array

```js
let hobbies = ["sport", "cooking", 25, true];

for (let hobby of hobbies) {
  console.log(hobbies[index]);
}

for (let hobby of hobbies) {
  console.log(hobby);
}

let hobbiesArray = ["sports", "cooking", "painting"];
let hobbiesSubset = hobbiesArray.slice(0, 2);
console.log(hobbiesSubset);

let filteredHobbies = hobbiesArray.filter((val) => val !== "sports");

let updatedHobbies = hobbiesArray.filter((val) => "hobby: " + val);
```

### Reference Types: primitive VS reference

- Reference types: Objects & Arrays
- Arrays
- 배열이 저장된 메모리 위치를 가리키는 주소
- 새 요소를 추가해도 주소는 변경되지 않음
- 배열을 변경할 때, 실제로 상수 값을 편집하는 것이 아니라 상수가 가리키는 값을 편집하는 것

```js
const hobbies = ["reading", "cooking"];

hobbies.push("programming");
// 상수로 선언 된 배열은 여전히 메모리의 같은 위치를 가리키고 있기 때문에 (reference type) 새로운 element 가 추가 된다
console.log(hobbies);

const hobbiesObject = {
  name: "Nata",
  type: "Programming",
};

// 객체 역시 reference type 이므로 상수로 선언 되어도 여전히 메모리의 같은 위치를 가리키고 있기 때문에 (reference type) 새로운 element 가 추가 된다
hobbiesObject.age = 26;
console.log(hobbiesObject);

// primitive type 인 상수에 다른 value 를 선언 하려고 하면 에러가 발생
const number = 12;
number = 11;
```

### Spread operator: 배열 또는 객체에서 모든 요소를 꺼내는 데 사용

```js
const originArray = [1, 2, 3, 4, 5];

// Use spread operator to create a new array with old values
const newArray1 = [...originArray];

const hobbies = ["sports", "cooking"];
const copiedHobbiesArray = [...hobbies, ...["traveling"]];

const person = { name: "Nata", age: 26 };
const copiedPerson = { ...person, ...{ hobby: "traveling" } };
```

### Rest Operator:

- 똑같이 '...' 로 표현
- 사용 장소에 차이점이 있음
- 함수에서 가변 arguments 를 다룰 때 유용하게 사용 / 매개변수가 가변적 일 때

```js
const toArray = (...args) => args;

console.log(toArray(1, 2, 3));
```

### Destructuring declarations (구조 분해 선언)

- 배열 및 객체 작업을 훨씬 더 간단하게 만들어주는 기능
- 데이터를 추출하는 방법
- 배열, 객체, 맵, 집합을 별개의 변수로 변환 가능
- 데이터를 추출하여 반복적이고 혼란스러운 코드를 피할 수 있고 복잡한 데이터 구조를 간결하고 체계적으로 정리 할 수 있다

```js
// Before Destructuring
const person = { name: "Nata", age: 26, occupation: "Dev" };
// const name = person.name;
// const age = person.age;
// const occupation = person.occupation;

const { name, age, occupation } = person;

console.log(name), occupation, age;
```

## Async, Await, Promises

### Async VS Sync

1. **동기(Synchronous)**

- 동기적인 작업은 순차적으로 실행됩니다. 즉, 한 작업이 완료되기 전에 다음 작업이 시작되지 않습니다.
- 코드가 위에서부터 아래로 차례대로 실행되는 것과 같은 개념입니다. 만약 어떤 작업이 시간이 오래 걸린다면, 다음 작업은 그 작업이 완료될 때까지 기다려야 합니다.

2. **비동기(Asynchronous)**:

- 비동기적인 작업은 순차적으로 실행되지 않습니다.
- 한 작업의 완료 여부와 상관없이 다음 작업이 시작될 수 있습니다. 이 때문에 다음 작업은 이전 작업이 완료되기를 기다리지 않고 동시에 실행될 수 있습니다.
- 비동기적인 작업은 보통 시간이 오래 걸리는 작업이나 외부 리소스를 이용할 때 유용합니다.

간단한 예로, 웹 페이지에서 이미지를 불러오는 작업을 생각해보세요. 동기적으로 처리하면 이미지가 로드될 때까지 다른 작업을 수행하지 못합니다. 그러나 비동기적으로 처리하면 이미지를 로드하는 동안에도 다른 작업을 계속할 수 있습니다.

```
동기작업이란 코드내 작업들이 차례대로, 그러니까 예를 들어 10줄의 코드가 있으면, 첫번째 코드가 끝나야 2번부터 10번까지 코드가 실행될 수 있는 작업방식을 말하고,

비동기작업이란 이런 작업과 다르게 1번 작업이 끝나지 않더라도 다음 작업들이 동시에 실행되는 작업방식이다. 예를 들어서, 앱에서 파일을 다운로드할 때 비동기 처리를 하면 다운로드가 완료되지 않더라도 다음 작업이 실행될 수 있는 거고, 여기서 다음 작업을 실행하기 위하여 특정 파일이 필요할 경우 await 구문을 붙여서 동기처리를 해주면 원하는 작업 로직을 구현 할 수 있다.
다운로드가 완료가 되어야 다음 작업이 처리될 수 있게 해준다

```

### Promise

- Promise는 비동기 작업의 최종 완료 또는 실패를 나타내는 객체입니다.

```javascript
const getUser = (username) => {
  const API_URL = `https://api.github.com/users/${username}`;
  // Fetch data from the constructed API URL
  return (
    fetch(API_URL)
      // Parse the response data as JSON
      .then((response) => response.json())
  );
};

// Call the getUser function with the username 'openai'
getUser("openai")
  // Once the data is fetched and parsed, log the result to the console
  .then((val) => console.log(val));
```

### Async/Await

- Async/await는 Promise를 더 쉽게 다룰 수 있도록 도와주는 문법입니다.
- async 함수는 항상 Promise를 반환합니다. 함수 내부에서 await 키워드를 사용하여 비동기 작업을 기다릴 수 있습니다.

```javascript
const getUser = async (username) => {
  const API_URL = `https://api.github.com/users/${username}`;
  const res = await fetch(API_URL);
  const data = await res.json();
  return data;
};

// Call the getUser function with the username 'openai'
getUser("openai")
  // Once the data is fetched and parsed, log the result to the console
  .then((val) => console.log(val));
```

### Promise with Error Handling

- Promise를 사용하여 비동기 작업을 수행하고, 작업이 성공하면 resolve를 호출하여 결과를 반환하고, 실패하면 reject를 호출하여 에러를 반환합니다.
- 에러 처리를 위해 reject를 사용하여 에러를 전달하고, 호출한 쪽에서 catch를 사용하여 에러를 처리합니다.

```javascript
const getGithubUser = (username) => {
  return new Promise((resolve, reject) => {
    fetch(`https://api.github.com/users/${username}`)
      // Parse the response data as JSON
      .then((response) => response.json())
      .then((data) => {
        // Check if the user is not found
        if (data.message === "Not Found") {
          // If user not found, reject the promise with an error message
          reject("User not found");
        } else {
          // If user found, resolve the promise with the user data
          resolve(data);
        }
      })
      // If there is any error during the process, reject the promise with the error
      .catch((err) => reject(err));
  });
};

// Call the getGithubUser function with the username 'jaydon'
// Log the user data if resolved, or log the error message if rejected
getGithubUser("jaydon")
  .then((val) => console.log(val))
  .catch((err) => console.log(err));
```
