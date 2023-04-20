---
title: "{ React Native } Image Picker"
date: 2023-04-19 15:00:00 +07:00
tags: [JavaScript, React Native, Image Picker]
---

- Expo ImagePicker
  - expo-image-picker 는 디바이스의 라이브러리에서 이미지와 비디오를 선택하거나 카메라로 사진을 찍을 수 있는 시스템의 UI에 대한 액세스를 제공함.
- Reference: https://docs.expo.dev/versions/latest/sdk/imagepicker/
```
npx expo install expo-image-picker
``` 

#### app.json config
```
{
  "expo": {
    "plugins": [
      [
        "expo-image-picker",
        {
          "photosPermission": "The app accesses your photos to let you share them with your friends."
        }
      ]
    ]
  }
}
```

#### Usage
```js
import React, { useState, useEffect } from 'react';
import { View, Image, StyleSheet, TouchableOpacity } from 'react-native';
import * as ImagePicker from 'expo-image-picker';
import CameraImage from '../../assets/images/camera_btn.png';

const styles = StyleSheet.create({
  imagesContainer: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  image: {
    width: 100,
    height: 100,
    margin: 5,
  },
});

const SellPage = () => {
  // State for storing the images
  const [images, setImages] = useState([]);

  // Function to pick an image from the media library
  const pickImage = async () => {
    // Request permission to access the media library
    const permissionResult = await ImagePicker.requestMediaLibraryPermissionsAsync();

    // If permission is not granted, show an alert and return
    if (!permissionResult.granted) {
      alert('Permission to access the camera roll is required.');
      return;
    }

    // Launch the image picker
    const pickerResult = await ImagePicker.launchImageLibraryAsync({
      mediaTypes: ImagePicker.MediaTypeOptions.Images,
      allowsEditing: true,
      aspect: [4, 3],
      quality: 1,
    });

    // If an image is selected, add it to the images array
    if (pickerResult.assets && pickerResult.assets.length > 0) {
      setImages([...images, pickerResult.assets[0].uri]);
    }
  };

  // Request permission to access the media library when the component mounts
  useEffect(() => {
    (async () => {
      const { status } = await ImagePicker.requestMediaLibraryPermissionsAsync();
      if (status !== 'granted') {
        alert('Sorry, we need camera roll permissions to make this work!');
      }
    })();
  }, []);

  // Render the component
  return (
    <View style={styles.imagesContainer}>
      {/* Camera image button to trigger the image picker */}
      <TouchableOpacity onPress={pickImage}>
        <Image source={CameraImage} style={{ width: 100, height: 100 }} />
      </TouchableOpacity>

      {/* Render the selected images */}
      {images.length
        ? images.map((imageUrl, index) => (
            <React.Fragment key={index}>
              <Image source={{ uri: imageUrl }} style={styles.image} />
            </React.Fragment>
          ))
        : ''}
    </View>
  );
};

export default SellPage;

```
