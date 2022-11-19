---
title: "{ React Native } How Does layout system works"
date: 2022-10-17 15:00:00 +07:00
tags: [JavaScript, React Native, Layout]
description: "The explanation how layout system works in React Native."
---

**How Does React Native Work**

### Layout system

- In React Native, flex container has flex direction of column, not row.
- Setting width and height is a bad idea because it looks different depends on the phone screen size (for responsive design).
- width and height are used ONLY for icons, or avatars.
- We use flex for the size of a component: proportion of size

```js
import { StyleSheet, View } from "react-native";

export default function App() {
  return (
    // Container
    <View style={styles.container}>
      <View style={styles.view1}></View>
      <View style={styles.view2}></View>
      <View style={styles.view3}></View>
    </View>
  );
}
// Size -> Number
const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  view1: {
    flex: 1,
    backgroundColor: "tomato",
  },
  view2: {
    flex: 3, // Means three time is bigger than 1
    backgroundColor: "lightblue",
  },
  view3: {
    flex: 1,
    backgroundColor: "green",
  },
});
```
