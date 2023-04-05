---
title: "{ Storage Machanisms } Array vs Local Storage"
date: 2023-04-04 10:30:00 +07:00
tags: [Storage Machanisms , Array, Local Storage]
---

### Array vs Local Storage

- Local storage is persistent, meaning that the data stored in it remains even after the user closes the browser or shuts down their computer. 
- Arrays, on the other hand, are stored in memory on the client-side and are not persisted across page reloads or when the user closes the browser.

### Using an array to persist data across different pages.

- It is possible but it has its limitations
- Arrays are a client-side storage mechanism and their values are stored in 'memory on the client-side'.
- When the user navigates away from the page or closes the browser window, the data stored in the array will be lost.
- However, you can use an array in combination with other client-side storage mechanisms such as cookies.
- Or server-side storage mechanisms like sessions.
- You can store the array as a JSON string in a cookie or session storage and retrieve it when the user navigates to a different page.

### Cookies

- Cookies are a client-side storage mechanism that allow web applications to store data on the user's computer.
- Data can be accessed by the same website or by a different website that shares the same domain.
- Cookies are typically used for storing small amounts of data that needs to be persisted across sessions or page reloads.
- Such as login credentials or shopping cart items.
- Cookies can be set and retrieved using JavaScript, and are supported by all modern web browsers.

```javascript
// Define an array to store the data
var myData = [];

// Add some data to the array
myData.push({ name: "John", age: 30 });
myData.push({ name: "Jane", age: 25 });
myData.push({ name: "Bob", age: 40 });

// Store the data in a cookie
document.cookie = "myData=" + JSON.stringify(myData);

// Retrieve the data from the cookie
var retrievedData = JSON.parse(getCookie("myData"));

// Make changes to the retrieved data
retrievedData[1].age = 27;

// Store the updated data back in the cookie
document.cookie = "myData=" + JSON.stringify(retrievedData);

// Output the updated data
console.log(retrievedData);

// Helper function to retrieve a cookie by name
const getCookie = (name) => {
  var value = ", " + document.cookie;
  var parts = value.split(", " + name + "=");
  if (parts.length == 2) return parts.pop().split(",").shift();
}
```

#### Explanation

- document.cookie 속성을 설정하여 데이터를 쿠키에 저장
- 'getCookie'라는 도우미 함수를 사용하여 쿠키에서 데이터를 검색하고 검색된 데이터를 변경하고 업데이트된 데이터를 다시 쿠키에 저장
- 쿠키에는 최대 4KB의 크기와 사용자가 비활성화할 수 있다는 limitation
- Requirements 에 따라 신중히 매커니즘을 선택 해야한다

### Sessions

- Sessions are a server-side storage mechanism that allow web applications to store user-specific data on the server
- Data can be accessed across multiple requests or sessions. 
- When a user starts a session, a unique session ID is generated, which can be used to identify the user's session on subsequent requests.
- Sessions have several advantages over client-side storage mechanisms, such as increased security, server-side control, and the ability to store larger amounts of data. 
- Server needs to be configured to support sessions, and the web application needs to use a server-side programming language, such as PHP, and Java.

```php
// Start the session
session_start();

// Define an array to store the data
$myData = array();

// Add some data to the array
$myData[] = array("name" => "John", "age" => 30);
$myData[] = array("name" => "Jane", "age" => 25);
$myData[] = array("name" => "Bob", "age" => 40);

// Store the data in the session
$_SESSION["myData"] = $myData;

// Retrieve the data from the session
$retrievedData = $_SESSION["myData"];

// Make changes to the retrieved data
$retrievedData[1]["age"] = 27;

// Store the updated data back in the session
$_SESSION["myData"] = $retrievedData;

// Output the updated data
print_r($retrievedData);

```