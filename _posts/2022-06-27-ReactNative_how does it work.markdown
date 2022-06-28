---
title: "{ React Native } How Does React Native Work"
date: 2022-06-27 17:00:00 +07:00
tags: [JavaScript, React Native, Components, API]
description: "The explanation how react native work with IOS and Android."
---

**How Does React Native Work**

### Cross-Platform with React Native
![ReactNative](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/react-native-introduction/en/resources/21.jpg)
- React JS works with Browser DOM.
- React Native does NOT work with Browser DOM, is a translater (interface) with IOS and Android.
- E.g. React Native will send a message to IOS or Android like saying "Please make a button".


### Just receiving and sending messages between JS and Native (OS).
![ReactNative_2](https://qph.cf2.quoracdn.net/main-qimg-b8c97bdb34956152ee1fab83c04e6b85-pjlq)
1. We are waiting for an event. Let's say it is where user will press a button on the screen.
2. This event will be recorded by Native side (Android and IOS); Detect all information about the event.
3. Bridge will send a JSON message.
4. JS receives that message.
5. Process the event, send it back to the native OS.
6. Serialized batched response (일괄 처리된 응답) 이 전달.
7. Process commands (e.g.배경색을 빨간색으로 해라).
8. Update UI.

### Creating the app with expo

- What is expo?
    - Expo is an open-source platform for making universal native apps for Android, iOS, and the web with JavaScript and React.

- Commands on Windows terminal
    - expo init firstReactApp
    - cd firstReactApp
    - npm start

- 윈도우에서 expo init 할때, CategoryInfo Security 오류 날 시,
1. Run as administrator
2. Set-ExecutionPolicy -ExecutionPolicy Unrestricted 
=> A (YES ALL)
3. expo init fileName

### Rapidly changing environment

- Components are built-in components for you to use in your app.
- You can use JavaScript to access your platform's API and we have use behaviors of UI using react components.
- Many Components and APIs have disappeared as it was hard to maintain all components and APIs.
- So, we have to rely on the community to create and maintain other Components.